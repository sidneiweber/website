---
title: Exportar e importar containers no docker
date: 2016-11-18T14:56:26-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/11/índice-220x159.png
tags: [docker]
---
### Exportando imagem

Temos aqui novamente um processo bem simples no docker, para exportar uma imagem uitlizamos o comando

```shell
docker save debian-apache > /tmp/imagem.tar
ls -lh /tmp/
-rw-r--r-- 1 root   root  225M nov 18 14:50 imagem.tar
```

### Importando imagem

```shell
docker load < imagem.tar
```

Fala a verdade é simples ou não é, mais fácil que isso não tem como.

[Fonte](http://devopslab.com.br/docker-como-criar-uma-imagem-docker-a-partir-de-um-container-utilizando-o-docker-commit/)