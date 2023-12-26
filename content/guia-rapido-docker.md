---
layout: page
name: Guia Rápido Docker
title: Guia Rápido Docker
author: Sidnei Weber
permalink: /guia-rapido-docker/
menu:
  main:
    name: Guia rápido Docker
    parent: guia-rapido
    params:
      icon: fa-brands fa-docker
---

# Guia rápido Docker

## Comandos Básicos
### Container
| Comando                         | Descrição                                                      |
|---------------------------------|----------------------------------------------------------------|
| `docker version`                | Verifica versão do Docker                                      |
| `docker container run`          | Executa container                                              |
| `docker container ls`           | Lista containers em execução                                   |
| `docker container inspect <ID>` |                                                                |
| `docker container ls -a`        | Lista containers, inclusive os que já encerraram               |
| `docker container stop <ID>`    | Para um container que está em execução                         |
| `docker container start <ID>`   | Inicia um container que está parado                            |
| `docker container restart <ID>` | Reinicia container                                             |
| `docker container rm <ID>`      |                                                                |
| `docker container rm -f <ID>`   |                                                                |
| `docker container top <ID>`     |                                                                |
| `docker container stats`        | Exibe informações sobre containers em execução (CPU, Mem, etc) |
| `docker container exec`         |                                                                |
| `docker container attach`       |                                                                |

### Imagens
| Comando                | Descrição                       |
|------------------------|---------------------------------|
| `docker image ls`      | Lista imagens presentes no host |
| `docker image inspect` |                                 |
| `docker pull`          |                                 |

## Rede

## Serviços
| Comando                  | Descrição |
|--------------------------|-----------|
| `docker service create`  |           |
| `docker service ls`      |           |
| `docker service inspect` |           |
| `docker service scale`   |           |
| `docker service ps`      |           |
| `docker service logs`    |           |
| `docker service rm`      |           |