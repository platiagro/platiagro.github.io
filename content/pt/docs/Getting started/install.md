---
title: Instalação da PlatIAgro
linkTitle: Instalação da PlatIAgro
weight: 20
description: >
  Veja as opções de instalação para cada ambiente.
---

### Instalação em um cluster Kubernetes

Para continuar é necessário ter um Cluster Kubernetes, [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) e [kfctl](https://github.com/kubeflow/kfctl/releases) configurados para acessar o cluster.

**ATENÇÃO: Atualmente, as versões 1.14 and 1.15 do Kubernetes são as únicas com suporte.**

Execute os seguintes comandos:

```
export KF_NAME=platiagro
export BASE_DIR=$(pwd)
export KF_DIR=${BASE_DIR}/${KF_NAME}
export CONFIG_URI="https://raw.githubusercontent.com/platiagro/manifests/v0.1.0-kubeflow-v1.0-branch/kfdef/kfctl_platiagro_tls.v0.1.0.yaml"
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
```

Então visite https://[LOAD-BALANCER-HOST]/

### Instalação no Google Kubernetes Engine (GKE)

Visite [https://platiagro-gcp.herokuapp.com/](https://platiagro-gcp.herokuapp.com/) e siga as instruções.
