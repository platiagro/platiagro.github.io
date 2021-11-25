---
title: Ambiente para Desenvolvimento
linkTitle: Ambiente para Desenvolvimento
weight: 20
description: >
  Informações de como usar o ambiente de PlatIAgro para desenvolvimento.
---

### 1. Expor o MinIO para o ambiente de desenvolvimento

No ambiente com a PlatIAgro, execute o seguinte comando para expor o MinIO:

```bash
kubectl patch svc nginx --patch \
   '{"spec": { "type": "NodePort", "ports": [ { "nodePort": 32001, "port": 9000, "protocol": "TCP", "targetPort": 9000 } ] } }'
```

No seu ambiente de desenvolvimento, defina as seguintes variáveis de ambiente:

```bash
export MINIO_ENDPOINT=<hostname-ou-endereco-ip>:32001
export MINIO_ACCESS_KEY=<access-key>
export MINIO_SECRET_KEY=<secret-key>
```

Obs: o access key e secret key podem ser obtidos com os seguintes comandos:

```bash
kubectl -n platiagro get secrets minio-secrets --template={{.data.MINIO_ACCESS_KEY}} | base64 -d
kubectl -n platiagro get secrets minio-secrets --template={{.data.MINIO_SECRET_KEY}} | base64 -d
```

### 2. Expor o MySQL para o ambiente de desenvolvimento

No ambiente com a PlatIAgro, execute o seguinte comando para expor o MySQL:

```bash
MYSQL=$(kubectl -n platiagro get pod -l app=mysql -o jsonpath={.items..metadata.name})
kubectl -n platiagro port-forward $MYSQL --address 0.0.0.0 31001:3306
```

Você pode combinar com `nohup kubectl -n platiagro port-forward $MYSQL --address 0.0.0.0 31001:3306 &` para deixar o processo rodando em segundo plano.

No seu ambiente de desenvolvimento, defina as seguintes variáveis de ambiente:

```bash
export MYSQL_DB_HOST=<hostname-ou-endereco-ip>:31001
export MYSQL_DB_USER=<user>
export MYSQL_DB_PASSWORD=<password>
```

Obs: a senha podem ser visualizada com o seguinte comando:

```bash
kubectl -n platiagro get secrets mysql-secrets --template={{.data.MYSQL_ROOT_PASSWORD}} | base64 -d
```

**ATENÇÃO 1: Expor com NodePort não funciona para o MySQL da PlatIAgro!**

**ATENÇÃO 2: O processo `kubectl -n platiagro port-forward ...` geralmente finaliza após alguns minutos/horas. É necessário rodar o comando novamente para que o MySQL fique acessível.**

### 3. Autenticar-se no Kubeflow Pipelines

O Kubeflow pipelines precisa do header `kubeflow-userid: anonymous@kubeflow.org` para executar operações. Este header é incluído automaticamente para serviços dentro do cluster. Para incluir o header em requisições externas (ex: num ambiente de desenvolvimento, localhost) crie o seguinte [envoy-filter](https://istio.io/latest/docs/reference/config/networking/envoy-filter/) no cluster:


```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: add-kubeflow-userid-filter
  namespace: istio-system
spec:
  workloadSelector:
    labels:
      istio: ingressgateway
spec:
  configPatches:
  - applyTo: VIRTUAL_HOST
    match:
      context: SIDECAR_OUTBOUND
      routeConfiguration:
        vhost:
          name: ml-pipeline.kubeflow.svc.cluster.local:8888
          route:
            name: default
    patch:
      operation: MERGE
      value:
        request_headers_to_add:
        - append: true
          header:
            key: kubeflow-userid
            value: anonymous@kubeflow.org
EOF
```

### 4. Desabilitar Autenticação (validação de cookie) em ambiente com Login

**Por que desabilitar a autenticação?**
- para que devs do Frontend acessem o backend da aplicação sem o cookie de login.
- para que devs do Backend acessem Kubeflow Pipelines a partir de fora do cluster
- para facilitar testes/integração de fluxos implantados

Siga as instruções [deste link](https://platiagro.github.io/docs/getting-started/skip-auth-url/) para desabilitar o login.

### 5. Aumentar os limites de *eviction* do Kubernetes para ambientes com pouco espaço em disco

No ambiente com a PlatIAgro, execute o seguinte comando:

```bash
echo 'KUBELET_EXTRA_ARGS=--eviction-hard=nodefs.available<1%,nodefs.inodesFree<1%,imagefs.available<1% --image-gc-high-threshold=99' | sudo tee /etc/default/kubelet
```

### 6. Trocar o HTTPS por HTTP no gateway de entrada

Por padrão a PlatIAgro é instalada com um certificado (HTTPS) auto-assinado e acessos HTTP redirecionam para HTTPS. 

Para desabilitar o HTTPS, execute os seguintes passos:

Editar o gateway `kubeflow-gateway` no namespace `kubeflow`:

```bash
kubectl -n kubeflow edit gateway kubeflow-gateway
```

Ao final do arquivo, você verá o seguinte trecho:

![Screenshot com exibição do comando que edita o gateway, antes de realizar as alterações.](/images/kubeflow-gateway.png)

Edite o `spec` para ficar da seguinte forma:

```yaml
...
spec:
  selector:
    istio: ingressgateway
  servers:
  - hosts:
    - '*'
    port:
      name: http
      number: 80
      protocol: HTTP
```

Após salvar as alterações, a plataforma estará acessível **apenas por http.** Acessos com HTTPS retornarão erro.

![Screenshot com exibição de acessos à plataforma com HTTP e HTTPS, após realizar as alterações.](/images/platiagro-http.png)


### 7. Habilitar CORS nos backends da PlatIAgro

Para que um desenvolvedor Frontend use o backend da aplicação, é necessário habilitar o [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

Edite os deployments `projects` e `datasets` no namespace `platiagro` e inclua a variável de ambiente `ENABLE_CORS=1` em ambos:

```bash
kubectl -n platiagro set env deployment/projects ENABLE_CORS=1
kubectl -n platiagro set env deployment/datasets ENABLE_CORS=1
```

### 8. Visualizar logs do backend da PlatIAgro

Para visualizar logs do **Istio Ingress Gateway**, execute o seguinte comando:

```bash
kubectl -n istio-system logs deployment/istio-ingressgateway --tail 100 -f
```

Para visualizar logs do backend [`projects`](https://github.com/platiagro/projects), execute o seguinte comando:

```bash
kubectl -n platiagro logs deployment/projects --tail 100 -f
```

Para visualizar logs do backend [`datasets`](https://github.com/platiagro/datasets), execute o seguinte comando:

```bash
kubectl -n platiagro logs deployment/datasets --tail 100 -f
```

Para visualizar logs do **Kubeflow Pipelines**, execute o seguinte comando:

```bash
kubectl -n kubeflow logs deployment/ml-pipeline --tail 100 -f
```


### 9. Visualizar Workflows e Logs de no Dashboard do Argo Workflows

É possível visualizar os logs e os status dos workflows no [dashboard](https://argoproj.github.io/argo-rollouts/dashboard/).

Esta interface é útil para DEBUG de problemas nos experimentos, deployments e tarefas.

![Screenshot com exibição do dashboard do argo workflows.](/images/argo-dashboard.png)

Execute o seguinte comando para adicionar o dashboard no ambiente da PlatIAgro:

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  labels:
    app.kubernetes.io/component: argo
    app.kubernetes.io/name: argo
    kustomize.component: argo
  name: argo-ui
  namespace: kubeflow
spec:
  gateways:
  - kubeflow-gateway
  hosts:
  - '*'
  http:
  - match:
    - uri:
        prefix: /argo/
    rewrite:
      uri: /
    route:
    - destination:
        host: argo-ui.kubeflow.svc.cluster.local
        port:
          number: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: argo-ui
    app.kubernetes.io/component: argo
    app.kubernetes.io/name: argo
    kustomize.component: argo
  name: argo-ui
  namespace: kubeflow
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: argo-ui
      app.kubernetes.io/component: argo
      app.kubernetes.io/name: argo
      kustomize.component: argo
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      creationTimestamp: null
      labels:
        app: argo-ui
        app.kubernetes.io/component: argo
        app.kubernetes.io/name: argo
        kustomize.component: argo
    spec:
      containers:
      - env:
        - name: ARGO_NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        - name: IN_CLUSTER
          value: "true"
        - name: ENABLE_WEB_CONSOLE
          value: "false"
        - name: BASE_HREF
          value: /argo/
        image: argoproj/argoui:v2.3.0
        imagePullPolicy: IfNotPresent
        name: argo-ui
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /
            port: 8001
            scheme: HTTP
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: argo
      serviceAccountName: argo
      terminationGracePeriodSeconds: 30
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: argo-ui
    app.kubernetes.io/component: argo
    app.kubernetes.io/name: argo
    kustomize.component: argo
  name: argo-ui
  namespace: kubeflow
spec:
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 31788
    port: 80
    protocol: TCP
    targetPort: 8001
  selector:
    app: argo-ui
    app.kubernetes.io/component: argo
    app.kubernetes.io/name: argo
    kustomize.component: argo
  sessionAffinity: None
  type: NodePort
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    application-crd-id: kubeflow-pipelines
  name: argo-cluster-role
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - pods/exec
  - pods/log
  verbs:
  - create
  - get
  - list
  - watch
  - update
  - patch
  - delete
- apiGroups:
  - ""
  resources:
  - configmaps
  verbs:
  - get
  - watch
  - list
- apiGroups:
  - ""
  resources:
  - persistentvolumeclaims
  verbs:
  - create
  - delete
  - get
- apiGroups:
  - argoproj.io
  resources:
  - workflows
  - workflows/finalizers
  verbs:
  - get
  - list
  - watch
  - update
  - patch
  - delete
  - create
- apiGroups:
  - argoproj.io
  resources:
  - workflowtemplates
  - workflowtemplates/finalizers
  - clusterworkflowtemplates
  - clusterworkflowtemplates/finalizers
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  resources:
  - serviceaccounts
  verbs:
  - get
  - list
- apiGroups:
  - argoproj.io
  resources:
  - cronworkflows
  - cronworkflows/finalizers
  verbs:
  - get
  - list
  - watch
  - update
  - patch
  - delete
- apiGroups:
  - ""
  resources:
  - events
  verbs:
  - create
  - patch
- apiGroups:
  - policy
  resources:
  - poddisruptionbudgets
  verbs:
  - create
  - get
  - delete
EOF
```

Acesse o dashboard do Argo Workflows no seguinte endereço: http://<hostname-ou-endedeco-ip-da-platiagro>/argo/
