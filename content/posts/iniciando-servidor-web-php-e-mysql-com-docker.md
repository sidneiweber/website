---
title: Iniciando servidor web PHP e Mysql com Docker
date: 2017-05-31T13:41:27-03:00
author: Sidnei Weber
layout: post
cover: /img/uploads/2017/05/Captura-de-tela_2017-05-31_13-34-26.png
tag: [docker, mysql, php, apache]
---
De uma maneira muito rápida podemos iniciar um servidor web para testarmos aplicações, páginas, sistemas, etc. Para isso precisaremos de duas ferramentas:

  * Docker
  * Docker Compose

Vou levar em consideração de já tenha os mesmos instalados, pois cada sistema tem seu próprio gerenciador de pacotes e não vou especificar isso no momento.

### Iniciando

#### DockerFile

Para iniciar criaremos um Dockerfile, para quem não está muito familiarizado pode ver um [post com comandos básico do docker aqui](http://sidneiweber.com.br/comandos-basicos-docker/). Usaremos uma imagem base do Docker Hub, a [tutum/lamp](https://hub.docker.com/r/tutum/lamp/).

```docker
FROM tutum/lamp
MAINTAINER PAAS EMAIL <email@site.com>
```

#### Docker Compose

Agora na mesma pasta iremos criar o arquivo **docker-compose.yml**. Com o conteúdo abaixo:

_Ps: Lembre de verificar se os caminhos dos arquivos estão corretos em seu sistema, pode variar de linux para linux._

```yaml
dev:
  dockerfile: Dockerfile
  volumes:
    - .:/var/www/html
    - /etc/timezone:/etc/timezone
    - /etc/localtime:/etc/localtime
 
  build: .
  expose:
    - "80"
 
  ports:
    - "80:80"
```

#### Subindo a aplicação

Subiremos a aplicação com o seguinte comando:

```bash
docker-compose up
```

Basta acessar seu localhost, ou ip de sua máquina que o servidor estará UP. A pasta onde foi criado os arquivos anteriores será a pasta raíz do servidor web. Ao iniciar será gerado uma saída parecido com a abaixo:

![docker](/img/uploads/2017/05/Captura-de-tela_2017-05-31_13-34-26-1024x233.png) 

[Fonte](http://blog.locaweb.com.br/artigos/desenvolvimento-artigos/docker-php-em-5-minutos/)