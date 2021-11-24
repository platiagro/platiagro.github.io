---
title: Criação de senha
linkTitle: Criação de senha para usuário da platiagro
weight: 40
description: >
  Veja como criar uma senha para um usuário da platiagro.
---

### Criação de login e senha na platiagro

É altamente __NÃO recomendado__ a utilização da senha predefinida do usuário padrão da platiagro em ambientes que precisam de segurança. Dito isto, segue abaixo as recomendações de criação de senha: 

1. considere o usuário padrão com o login `platiagro`, basta escolher uma senha para esse usuário e em seguida transformá-la em uma hash utilizando a lib `bcrypt` do python, segue os passos para realizar isto: 

Comando de instalação de bibliotecas necessárias(bcrypt e passlib):
```
pip3 install passlib[bcrypt]
pip3 install bcrypt

obs: Para instalações de bibliotecas python 3 é necessário ter o gerenciador de pacotes pip3 instalado 

```
Script de geração da hash: 
```
python3 -c 'from passlib.hash import bcrypt; import getpass; print(bcrypt.using(rounds=12, ident="2y").hash(getpass.getpass()))'

obs: Será solicitada a senha que deseja ser transformada em hash. O comando acima foi executado em terminais linux com interpretador de python(>=3.6).

```
segue um exemplo de utilização do bcrypt para geração de hash: ![Hash generation file](/images/hash_generation.png)

2. A hash gerada no passo anterior precisa ser inserida em `dex/base/config-map.yaml`. É possível usar um comando do kubectl para editar o configmap, segue o comando: 

```
kubectl -n auth edit configmap dex

```
segue um exemplo de trecho do configmap: ![Config map file](/images/configmap.png)

3. Uma vez com o configmap aberto, basta inserir o email/hash no seguinte trecho do arquivo:

```
...
  staticPasswords:
  - email: platiagro
    hash: <enter the generated hash here>

```  

Exemplo de mais de um email/hash configurado: ![Config email hash](/images/config-email-hash.png)

4. Reinicie o serviço para que as novas senhas comecem a ser utilizadas, utilizando o seguinte comando:
  
```
kubectl -n auth delete pod -l app=dex

``` 
### Links úteis
- Documentação do dex: https://dexidp.io/docs/using-dex/
- Instruções sobre instalações de pacotes python: https://packaging.python.org/tutorials/installing-packages/
- Repositório do bcrypt: https://github.com/pyca/bcrypt/ 
- Tutorial sobre encriptação de senhas com bcrypt: https://medium.com/py-bcrypt/encriptando-senhas-em-python-com-bcrypt-25e46b5c8166
