---
title: Visão Geral
linkTitle: Visão Geral
weight: 10
description: >
  Veja como a PlatIAgro utiliza diversas tecnologias para construir uma
  plataforma de IA.
---

![Visão macro de arquitetura da PlatIAgro.](/images/architecture-1.png)

![Visão macro de arquitetura da PlatIAgro.](/images/architecture-2.png)

O diagrama a seguir mostra os diversos componentes que compõem a PlatIAgro:

![Diagrama de componentes da PlatIAgro.](/images/platiagro-components.png)

### [Web UI](https://github.com/platiagro/web-ui)
Interface gráfica da PlatIAgro que dá acesso às funcionalidades da plataforma.

### [Projects](https://github.com/platiagro/projects)
API de gestão de projetos, experimentos e tarefas.

### [Datasets](https://github.com/platiagro/datasets)
API de gerenciamento de conjuntos de dados.

### [Kubeflow Pipelines](https://github.com/kubeflow/pipelines)
Platforma para construção e execução de fluxos de *machine learning* com base em containers Docker.

### [Argo Workflows](https://argo-cd.readthedocs.io/en/stable/)
Motor de orquestração de cargas de trabalho no Kubernetes.

### [Kubeflow Notebook Controller](https://github.com/kubeflow/kubeflow/tree/master/components/notebook-controller)
Serviço de gestão de ambientes de Notebook (ex: Jupyter).

### [Kubeflow Access Management](https://github.com/kubeflow/kubeflow/tree/master/components/access-management)
Serviço de controle de acesso usuário-namespace. Permite uma arquitetura "multi-tenant" para componentes do Kubeflow.

### [Kubeflow Profile Controller](https://github.com/kubeflow/kubeflow/tree/master/components/profile-controller)
Serviço controlador de perfis com base em Kubernetes RBAC e Istio AuthorizationPolicy.

### [Seldon Core](https://github.com/SeldonIO/seldon-core)
Plataforma para implantação de modelos de *machine learning* em serviços REST prontos para production.

### [KNative](https://knative.dev/)
Plataforma para implantação de cargas *serverless*.

### [Jupyter](https://jupyter.org/)
Ambiente interativo para desenvolvimento de código e análise de dados.

### [MySQL](https://www.mysql.com/)
Sistema de gerenciamento de banco de dados relacional.

### [MinIO](https://min.io/)
Serviço de armazenamento de arquivos compatível com Amazon S3.

### [Istio](https://istio.io/latest/)
Gerenciador de microsserviços com base em Kubernetes.

### [Dex OIDC](https://dexidp.io/)
Serviço de autenticação com base no protocolo OpenID Connect.
