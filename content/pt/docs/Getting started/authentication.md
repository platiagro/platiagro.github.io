---
title: Autenticação PlatIAgro
linkTitle: Autenticação PlatIAgro
weight: 30
description: >
    Autenticação para usar modelos implatados em ambiente com login.
---

O método para obter o token authservice_session para enviar solicitações de predição autenticadas ao PlatIAgro.

### Usando Python

Criar um arquivo, por exemplo, `authpla.py`, e colocar o conteúdo abaixo:

```
import requests
import os

requests.packages.urllib3.disable_warnings(
    requests.packages.urllib3.exceptions.InsecureRequestWarning)

USERNAME = "platiagro"
PASSWORD = "platiagro1234"
HOST = os.popen("kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.clusterIP}'").read()
NAMESPACE="istio-system"

session = requests.Session()
response = session.get(f"https://{HOST}", verify=False)

headers = {
    "Content-Type": "application/x-www-form-urlencoded",
}

data = {"login": USERNAME, "password": PASSWORD}
session.post(response.url, headers=headers, data=data)
print(session.cookies.get_dict())
session_cookie = session.cookies.get_dict()["authservice_session"]
print(session_cookie)
```


Executar o script `python3 authpla.py`:
- Retorno esperado:

```
{'authservice_session':'MTYzMDUyNjMzM3xOd3dBTkZaVVVsazNWRFEwUlRSWFRGUkZOak5FU1RWV1MxVTBVbGhMVDBwTFRsZzNSa1ZEV1UxV1JWRTBSVXRGVUZGSU0weFpNMEU9fNUnZGVaU65-l1deO2fW6CZlMEZVcpcmXMqInjV6EFj3'}
```
