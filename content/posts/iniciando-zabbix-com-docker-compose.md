---
title: Iniciando Zabbix com Docker Compose
date: 2018-09-06T23:16:18-03:00
author: Sidnei Weber
layout: post
#cover: /assets/img/uploads/2017/10/zabbix.png
tags: [zabbix]
---
Obviamente precisaremos ter instalado o docker e docker-compose, caso não saiba como instalar pode acessar o [link](https://docs.docker.com/install/) e esse outro [link](https://docs.docker.com/compose/install/). Precisaremos criar um arquivo **docker-compose.yml** com o seguinte conteúdo:

```yaml
persistentDB:
  img "busybox:latest"
  volumes:
    - /var/lib/mysql
zabbixDB:
  img "monitoringartist/zabbix-db-mariadb"
  environment:
    MARIADB_USER: zabbix
    MARIADB_PASS: senha
    PHP_date_timezone: America/Sao_Paulo
    ZS_StartDiscoverers: "5"
    ZS_StartPingers: "5"
  volumes_from:
    - persistentDB
  volumes:
    - "/etc/localtime:/etc/localtime:ro"
    - "/srv/zabbix/backups:/backups"
  hostname: zabbix-db
zabbix:
  img "monitoringartist/zabbix-3.0-xxl:latest"
  ports:
    - "80:80"
    - "10051:10051"
  volumes:
    - "/etc/localtime:/etc/localtime:ro"
  links:
    - "zabbixDB:zabbix.db"
  environment:
    ZS_DBHost: zabbix.db
    ZS_DBUser: zabbix
    ZS_DBPassword: senha
    TERM: xterm
```

E iremos executar o seguinte comando:

```shell
docker-compose up -d
```

Caso queiram contribuir segue o github desse código: [https://github.com/sidneiweber/zabbix-docker](https://github.com/sidneiweber/zabbix-docker)
