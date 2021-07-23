---
title: Instalando um dashboard para seu servidor linux
description: Instalando um dashboard para seu servidor linux
date: 2014-02-04T23:31:35-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
  - servidores
---
![dash ><](/img/uploads/2014/02/screen-shot-2014-01-28-at-11-09-031.png)

```shell
cd /var/www/
git clone https://github.com/afaqurk/linux-dash.git
yum -y install php php-common php-gd php-mbstring php-xml php-xmlrpc
/etc/init.d/httpd start
```

Agora acesse: http://endereco.ip.do.server/linux-dash

[Fonte](http://www.ricardomartins.com.br/dashboard-lindao-para-seu-servidor-linux/)