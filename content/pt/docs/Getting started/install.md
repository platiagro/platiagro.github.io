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

Há 4 modos de instalação:

```
platiagro           sem autenticação e pronta para CPU.
platiagro-gpu       sem autenticação e pronta para GPU.
platiagro-auth      com autenticação e pronta para CPU.
platiagro-gpu-auth  com autenticação e pronta para GPU.
```

Ajuste a variável `INSTALL_DIR=platiagro`, e então rode os seguintes comandos:

```
export INSTALL_DIR=platiagro
git clone --single-branch --branch v0.2.0-kubeflow-v1.3-branch https://github.com/platiagro/manifests.git
cd manifests
while ! kustomize build $INSTALL_DIR | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

Então visite https://[LOAD-BALANCER-HOST]/

### Instalação no Google Kubernetes Engine (GKE)

Visite [https://platiagro-gcp.herokuapp.com/](https://platiagro-gcp.herokuapp.com/) e siga as instruções.
