---
title: Iniciando servidor apache no docker
date: 2016-11-17T10:59:38-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/11/índice-220x159.png
tags:
  - docker
---
Para iniciar um servidor apache no docker é muito simples, caso tenha uma imagem que já tenha apache é mais simples ainda. Mas vamos partir do principio que não tenha essa imagem, usaremos uma imagem do repositório do docker.

Caso queira só baixar a imagem, começaremos com o comando:

```shell
docker pull eboraas/apache
```

Para iniciar o container com nosso servidor rodaremos o comando:

```shell
docker run -it -p 80:80 -d eboraas/apache
```

Caso queira iniciar também expondo as portas para ter suporte SSL:

```shell
docker run -it -p 80:80 -p 443:443 -d eboraas/apache
```

Basta acessar o IP de sua máquina que o servidor já estará rodando. Caso queira usar algum projeto web que tenha em sua máquina podemos linkar essa pasta com a pasta do container.

```shell
docker run -it -p 80:80 -v /home/sidnei/meusite/:/var/www/html/ -d eboraas/apache
```

Se caso a porta 80 de seu host esteja sendo usada, é possível mapear outra porta no container. Bastando acessar o IP:8080 para ter acesso ao apache do container.

```shell
docker run -it -p 8080:80 -v /home/sidnei/meusite/:/var/www/html/ -d eboraas/apache
```

[Fonte](https://hub.docker.com/r/eboraas/apache/)