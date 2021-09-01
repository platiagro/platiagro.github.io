---
title: Autenticação Platiagro
linkTitle: Autenticação Platiagro
weight: 30
description: >
    Autenticação para usar modelos implatados em ambiente com login.
---

Existem 2 métodos para obter o token authservice_session para enviar solicitações de predição autenticadas ao PlatIAgro.

### Usando Python

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

