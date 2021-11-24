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

**ATENÇÃO: Export por NodePort não funciona para o MySQL da PlatIAgro!**

### 3. Autenticar-se no Kubeflow Pipelines

No ambiente com a PlatIAgro, execute o seguinte comando para adicionar um header de usuário para todas requisições externas ao Kubeflow Pipelines:


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

Siga as instruções [deste link](https://platiagro.github.io/docs/getting-started/skip-auth-url/).

### 5. Aumentar os limites de *eviction* do Kubernetes

No ambiente com a PlatIAgro, execute o seguinte comando:

```bash
echo 'KUBELET_EXTRA_ARGS=--eviction-hard=nodefs.available<1%,nodefs.inodesFree<1%,imagefs.available<1% --image-gc-high-threshold=99' | sudo tee /etc/default/kubelet
```
