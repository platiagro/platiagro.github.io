---
title: Instalação da PlatIAgro
linkTitle: Instalação da PlatIAgro
weight: 20
description: >
  Veja as opções de instalação para cada ambiente.
---

### Instalação em um cluster Kubernetes

Certifique-se de ter os seguintes pré-requisitos antes de instalar a PlatIAgro:

- `Kubernetes` (testado com a versão `1.18`) com um [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/)
- `kustomize` (testado com a versão `3.2.0`) ([download link](https://github.com/kubernetes-sigs/kustomize/releases/tag/v3.2.0))
    - PlatIAgro é construída usando o [Kubeflow](https://www.kubeflow.org), e o Kubeflow 1.3.0 não é compatível com a versão mais recente do kustomize 4.x.
- `kubectl`

Execute os seguintes comandos:

```
git clone --single-branch --branch v0.2.0-kubeflow-v1.3-branch https://github.com/platiagro/manifests.git
cd manifests
while ! kustomize build platiagro | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

Então visite https://[LOAD-BALANCER-HOST]/

### Instalação no Google Kubernetes Engine (GKE)

Visite [https://platiagro-gcp.herokuapp.com/](https://platiagro-gcp.herokuapp.com/) e siga as instruções.
