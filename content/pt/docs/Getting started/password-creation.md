---
title: Criação de senha
linkTitle: Criação de senha para usuário kubeflow
weight: 20
description: >
  Veja como criar uma senha para um usuário do Kubeflow.
---

### Criação de login e senha na platiagro

É altamente __NÃO recomendado__ a utilização da senha predefinida do usuário padrão do Kubeflow em ambientes que precisam de segurança. Dito isto, segue abaixo as recomendações de criação de login e senha: 

- 1° considere o usuário padrão com o email `user@example.com`, basta escolher uma senha para esse usuário e em seguida transformar em uma hash utilizando a lib de python `bcrypt`, segue um trecho de código capaz de realizar isto: 

```
python3 -c 'from passlib.hash import bcrypt; import getpass; print(bcrypt.using(rounds=12, ident="2y").hash(getpass.getpass()))'

```
- 2° A hash gerada no passo anterior precisará ser inserida em `dex/base/config-map.yaml`. É possível usar um comando do kubectl para editar o configmap, segue o comando: 

```
kubectl -n auth edit configmap dex

```
- 3° Reinicie o serviço e use o configmap utilizando o seguinte comando
  
```
kubectl -n auth delete pod -l app=dex

``` 
