---
title: '[Dica rápida] Meus 10 comandos mais rodados no linux'
description: Com este comando podemos conferir quais os 10 comandos mais rodados em nossa máquina
date: 2016-11-28T15:39:52-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2015/07/cropped-captura_de_tela-220x162.jpg
tags:
  - linux
---
Com este comando podemos conferir quais os 10 comandos mais rodados em nossa máquina:

```shell
history|awk '{print $2}'|awk 'BEGIN {FS="|"} {print $1}'|sort|uniq -c|sort -rn|head -10
```

Meu resultado foi:

```shell
history|awk '{print $2}'|awk 'BEGIN {FS="|"} {print $1}'|sort|uniq -c|sort -rn|head -10
    248 sudo
    148 vi
    101 sh
     49 ping
     49 ls
     40 systemctl
     38 ifconfig
     38 htop
     31 ps
     26 du
sidnei@black:~$
```