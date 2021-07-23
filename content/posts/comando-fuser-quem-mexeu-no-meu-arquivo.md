---
layout: post
title: Comando fuser - Quem mexeu no meu arquivo
date: '2018-11-06 09:28:53 -0300'
author: Sidnei Weber
#cover: "/img/fuser.png"
tags:
  - linux
---

O **fuser** é um programa que permite que saibamos qual processo está utilizando determinado arquivo, socket (portas) e sistema de arquivos especificado. Aprender sua manipulação é essencial para poder administrar um servidor para saber o que está acontecendo principalmente nas conexões. É um comando extremamente flexível, vamos ver suas opções e seu uso.

| Diretiva   | Descrição   |  Exemplo |
|:----------:|:-------------:|:------:|
| -a, --all | Mostra todos os arquivos, inclusive os que estão sem uso | fuser -a * |
| -k, --kill | Desativa/Mata os processos que estão utilizando determinado arquivo  | fuser -k /home/zonebin |
| -i, --interactive | Pede confirmação sempre que for matar um processo utilizando um arquivo | fuser -ik /home/zonebin |
| -m, --mount | Especifica um sistema de arquivos para descobrir qual processo está sendo utilizado | fuser -m /dev/sda1 |
| -s, --silent | Realiza as operações indicadas silenciosamente, não use a opção -a, -u, -v | fuser -ks /home |
| -u, --user | Mostra o nome de usuário que iniciou o processo que está utilizando o arquivo | fuser -u /var/log/messages |
| -4, --ipv4 | Mostra processos de IPV4 somente | fuser -4 ssh/tcp -6 |
| -ipv6 | Mostra somente processos de sockets IPV6 | fuser -6 25/tcp |

**Tipos de acesso:**  
c  Diretório atual  
e  Arquivo executável rodando  
f  Arquivo aberto (omitido no modo de display padrão)  
F  arquivo aberto para escrita (omitido no modo de display padrão)  
r  Diretório root  
m  Arquivo mapeado ou biblioteca compartilhada

**Exemplos:**
Mostrar os processos em execução no diretório atual

```shell
fuser -v .
```

Verificando se está sendo usado socket TCP ou UDP, como a porta 22 (SSH):

```shell
fuser -v -n tcp 22
```

Para mais informações:

```shell
man fuser
```
