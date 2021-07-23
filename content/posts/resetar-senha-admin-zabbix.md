---
title: Resetar senha Admin Zabbix
date: 2017-10-17T09:44:49-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2017/10/zabbix.png
tags: [zabbix]
---
Hoje vamos a uma dica rápida para quem esqueceu ou não sabe a senha de Admin do painel de administração do Zabbix. Eu estava testando a ferramenta e depois de um tempo se mexer havia esquecido a senha, tive que resetá-la. Para isso a única coisa que precisamos é ter a senha de root do mysql, tendo ela podemos logar no terminal:

```shell
mysql -u root -p
Enter password:
```

Dentro do terminal a gente vai fazer o seguinte:

```shell
mysql> use zabbix;
mysql> UPDATE users SET passwd=md5(‘novasenha’) WHERE alias=’Admin’;
```

Pronto, senha resetada.

[Fonte:](https://jorgepretel.com.br/2016/04/reset-na-senha-admin-do-zabbix/)