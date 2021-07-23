---
title: Bloquear resposta ao ping com sysctl
description: Bloquear resposta ao ping com sysctl
date: 2017-10-20T22:21:16-03:00
author: Sidnei Weber
layout: post
tags: [linux, rede]
---
A dica de hoje é super rápida e prática, para bloquear as respostas de ping podemos usar o comando sysctl:

Para bloquear:

```shell
sysctl -w net.ipv4.icmp_echo_ignore_all=1
```

Para desbloquear:

```shell
sysctl -w net.ipv4.icmp_echo_ignore_all=0
```
