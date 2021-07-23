---
layout: post
title: Backup Mysql - Linha comando
date: '2015-07-06 15:48:02 -0300'
author: Sidnei Weber
tags:
- linux
- mysql
---

Backup de uma base específica

```shell
mysqldump --database -d NOME DA BASE DE DADOS -u USUARIO -p > c:meu_db.sql
```

Backup de todas as bases

```shell
mysqldump --all-databases -u USUARIO -p > meu_mysql.sql
```