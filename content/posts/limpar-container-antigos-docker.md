---
id: 662
title: Limpar container antigos Docker
date: 2018-12-26T11:48:35-03:00
author: Sidnei Weber
layout: post
#cover: /img/docker.png
tags: [docker]
---
Caso sua lista de container esteja muito grande e queira remover alguns containers do seu host, podemos usar o comando abaixo para remover container parados a mais tempo:

```shell
docker ps --filter "status=exited" | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm
```

Explicando:

```shell
docker ps --filter "status=exited"
```
Lista somente os containers parados, que não estão em execução

```shell
grep 'weeks ago'
```
Filtra por containers criados a semanas atrás

```shell
awk '{print $1}'
```
Exibe a primeira coluna, que refere ao CONTAINER ID

```shell
xargs --no-run-if-empty docker rm
```
Pega o que foi filtrado até agora e joga como parâmetro para o _docker rm_

Sempre use os comandos com muito cuidado caso não tenha certeza do que está fazendo, não me responsabilizo por qualquer erro humano ;).
