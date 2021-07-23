---
title: Colocando uma firula no terminal
date: 2015-07-06T15:48:53-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
  - shell script
---
Editar o arquivo .bashrc e adicionar a linha a seguir

```shell
#PS1='[u@h W]$ '
PS1='┌─[u@h W][e[0;32m][${cwd}t][<wbr />033[0m] ${fill}n[\033[0m]└─■ '
```