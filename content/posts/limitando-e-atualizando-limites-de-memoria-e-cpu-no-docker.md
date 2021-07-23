---
title: Limitando e atualizando limites de memória e CPU no docker
description: Limitando e atualizando limites de memória e CPU no docker
date: 2017-06-13T13:55:20-03:00
author: Sidnei Weber
layout: post
tags: [docker]
---

Bom hoje vamos seguir com nosso aprendizado em docker, já vimos sobre [comandos básicos](http://sidneiweber.com.br/comandos-basicos-docker/), [iniciar servidor apache](http://sidneiweber.com.br/iniciando-servidor-apache-no-docker/), [exportar e importar containers](http://sidneiweber.com.br/exportar-e-importar-containers-no-docker/) e agora a dica é bem simples porém muito útil. Toda vez que subimos um container sem colocar limites nos recursos, o container pode usar todo o recurso da máquina fisica, isso nem sempre é bom, seja por onerar o host ou mesmo para testes da sua aplicação. Então vamos as dicas.

Antes da dica, vamos a outra dica :), com o comando _**docker stats**_ podemos ver o consumo de nossos containers:

![docker](/img/uploads/2017/06/Captura-de-tela_2017-06-13_13-47-34-1024x79.png)

Agora vamos as nossas dicas de hoje.

#### Limitar memória do container

```shell
# Para limitar a 512 MEGAS
docker run -it -m 512M ubuntu /bin/bash
#Para limitar a 1 GIGA
docker run -it -m 1G ubuntu /bin/bash
# Verificando a quantidade de memória
docker inspect [container id] |grep -i mem
```

#### Limitar CPU do container

```shell
docker run -it --cpu-shares 1024 ubuntu /bin/bash
docker inspect [container id] |grep -i cpu
```

### Atualizar limites de container em execução

#### Alterando limite de memória

```shell
docker update -m 256M [container id]
```

#### Atualizar limite CPU

```shell
docker update --cpu-shares 512 [container id]
```

Sempre lembrando que para pegar o id do container basta executar _**docker ps**_.
