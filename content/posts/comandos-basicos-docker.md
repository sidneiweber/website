---
title: Comandos básicos Docker
date: 2016-11-17T10:33:43-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/11/índice-220x159.png
tags:
  - docker
  - servidores
---
Docker é um ferramenta que venha aprendendo a pouco tempo, não explicarei o que é o docker, apenas alguns detalhes no uso. Caso queira uma explicação melhor sobre o que é docker, recomendo esse [artigo do Mundo Docker](http://www.mundodocker.com.br/o-que-e-docker/).

Segue abaixo alista dos comandos mais básicos e explicações básicas sobre o docker:

```shell
docker search ubuntu # Procura versões do sistema ubuntu
docker pull [nome da imagem] # baixar imagem
docker images # listar imagens
docker run [nome da imagem ou id] # iniciar container com a imagem baixada
docker ps # listar containers
docker ps -a # Verifica todos os containers, inclusive os que estão parados
docker exec [id do container] [comando] # executa comandos no container
docker rm [id do container]
```

Iniciar container com alguns detalhes a mais:

```shell
docker run -it -p <porta_host>:<porta_container> --name <nome_container> <nome_imagem>
```

Sendo que o -i significa interatividade e o -t que queremos um link com o terminal do container.

Iniciar uma sessão bash em um container que já esteja rodando:

```shell
docker exec -it <nome_container> bash

Verificar os logs de um container:

```shell
docker logs <nome_container>
```

Remover todos os containers parados:

```shell
docker rm -f $(docker ps -a -q)
```

Remover uma imagem baixada:

```shell
docker rmi -f <nome_imagem>
```

Copiar um arquivo do container para o host:

```shell
docker cp <nome_container>:/caminho/no/container /caminho/no/host
```

## Salvando alterações de um container modificado

Após instalar alguns programas ou fazer modificações no seu container, é possível que queira salvá-lo para não perder essas alterações. Para isso existe a opção _commit_ do docker que irá gerar uma nova imagem do seu container com as alterações. Pegaremos como base o ID do nosso container:

```shell
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
f8d74ee7155a        7e616530e7ea        "/bin/bash"         12 minutes ago      Up 12 minutes       0.0.0.0:8080-&gt;80/tcp   lonely_mccarthy
root@black:/home/sidnei#
```

Com nosso ID em mãos faremos o commit:

```shell
docker commit f8d74ee7155a debian-apache
sha256:9a232ff09aaf601dfbe487c3b20802079cd5e1a55115fb3a47473f9e44ab8bf0
root@black:/home/sidnei# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
debian-apache         latest              9a232ff09aaf        4 seconds ago       226.9 MB
```

## Dockerfile

Um Dockerfile é um script que automatiza a criação de imagens docker. Veja alguns exemplos de comandos que podem ser utilizados no Dockerfile. Em outros posts trarei com mais detalhes como cada um funciona.

#### FROM

Primeira instrução, define a imagem base.

```docker
FROM ubuntu14:04
```

#### MAINTAINER

Especifica o autor da imagem.

```docker
MAINTAINER Foo foo@bar.com
```

#### RUN

Equivalente ao comando docker run.

```docker
RUN apt-get install python
```

#### ENV

Define uma variável de ambiente.

```docker
ENV PORT=8000
```

#### EXPOSE

Expõe portas.

```docker
EXPOSE 8000
```

#### ADD

Copia arquivos do host hospedeiro para dentro da imagem.

```docker
ADD foo.txt /bar/foo.txt
```

#### ENTRYPOINT

Permite que a imagem seja executada como uma aplicativo (a partir da linha de comando especificada).

```docker
ENTRYPOINT ["python", "app.py"]
```

#### CMD

Comando que será executado quando a execução do container for acionada.

```docker
CMD ["supervisord"]
```

#### Exemplo de dockerfile

```docker
FROM debian:lastest

MAINTAINER Sidnei Weber <sidnei.weber@gmal.com>

RUN apt-get update
RUN apt-get install -y apache2
RUN /etc/init.d/apache2 start
```

Para gerar a imagem a partir do nosso dockerfile usaremos o docker build. Lembrando que se criamos o docker file em alguma pasta especifica, deveremos estar dentro desta pasta para executar o comando a seguir:

```docker
docker build -t teste .
```

Por hoje era isso pessoal, em breve estaremos estudando mais sobre o assunto.

Fonte: [Diego Garcia](http://www.diego-garcia.info/2015/02/15/docker-por-onde-comecar/)