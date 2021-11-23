---
title: Desabilitar Autenticação
linkTitle: Desabilitar Autenticação
weight: 60
description: >
  Mais informações para sobre a autenticação na PlatIAgro.
---

### 1. Como "desabilitar" a autenticação para certas URLs?

Certas URLs não precisam do cookie de autenticação, ou podem ser temporariamente liberadas (para desenvolvimento, por exemplo).

Para isso deve-se editar o configmap `oidc-authservice-parameters` no namespace `istio-system`:

```
kubectl -n istio-system edit configmap oidc-authservice-parameters
```

![Screenshot com exibição do comando que edita o configmap, antes de realizar as alterações.](/images/oidc-authservice-parameters.png)

Edite o valor `SKIP_AUTH_URI` com as URLs que você deseja que ignorem a autenticação. A imagem abaixo mostra um exemplo para as URLs dos serviços projects e datasets. Você pode encontrar mais detalhes sobre esta configuração [neste link](https://github.com/yanniszark/oidc-authservice).

![Screenshot com exibição do comando que edita o configmap, após realizar as alterações.](/images/oidc-authservice-parameters-updated.png)

Após editar os valores, aplique as mudanças reiniciando o serviço `authservice-0` no namespace `istio-system`:

```
kubectl -n istio-system delete pod authservice-0
```

![Screenshot com exibição do comando que reinicia o serviço.](/images/oidc-authservice-parameters-restart.png)

PRONTO! Você pode verificar que as URLs estão acessíveis mesmo sem o cookie de autenticação.

![Screenshot com exibição do comando que reinicia o serviço.](/images/skip-auth-uri.png)
