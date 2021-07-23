---
title: Gerar lista de pacotes instalados no Debian, Ubuntu, etc
date: 2017-06-22T13:22:53-03:00
author: Sidnei Weber
layout: post
permalink: gerar-lista-de-pacotes-instalados-no-debian-ubuntu-etc/
tags:
  - dpkg
  - linux
---
Para gerar uma lista dos pacotes instalados no sistema, poderemos usar o **dpkg** para isso. Pode ser muito útil na hora de criar sistemas com a mesma base

#### Gerando lista de pacotes

```shell
sudo dpkg --get-selections > install.list
```

#### Instalando pacotes a partir da lista

```shell
sudo dpkg --set-selections < install.list
sudo apt-get -y update
sudo apt-get dselect-upgrade
```

[Fonte](http://www.dicas-l.com.br/dicas-l/20161025.php)