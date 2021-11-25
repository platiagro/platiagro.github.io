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


### 7. Habilitar [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) nos backends da PlatIAgro

Para que um desenvolvedor Frontend use o backend da aplicação, é necessário habilitar o [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

Edite os deployments `projects` e `datasets` no namespace `platiagro` e inclua a variável de ambiente `ENABLE_CORS=1` em ambos:

```bash
kubectl -n platiagro set env deployment/projects ENABLE_CORS=1
kubectl -n platiagro set env deployment/datasets ENABLE_CORS=1
```
