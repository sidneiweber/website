---
title: Comando para saber quando foi instalado o seu Linux
date: 2013-11-20T19:29:00-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
---

```shell
ls -lct /etc | tail -1 | awk '{print $6, $7, $8}'
```