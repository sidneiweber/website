---
title: Configurar sudo sem senha
date: 2017-12-01T21:29:13-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/12/Selecao_003.png
tags: [linux, sudo]
---
Para configurar o uso do uso sem senha, o que não é recomendado, somente em casos de algum programa precise de permissão para executar determinado comando. Podemos usar como exemplo o Zabbix que quer rodar algum programa com privilégio de root sem comprometer a segurança. O arquivo a ser editado será o /etc/sudoers:

Configurar sudo sem senha para tudo:

```shell
zabbix    ALL=NOPASSWD: ALL
```

Ou podemos configurar para executar somente algo em específico:

```shell
zabbix ALL=NOPASSWD: /usr/bin/nmap
# Mais de um comando
zabbix ALL=NOPASSWD: /usr/bin/nmap, /etc/init.d/apache2 restart
```

E por hoje era isso pessoal, aquele abraço
