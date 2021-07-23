---
title: Hora mais legível no dmesg
description: Como deixar a hora mais legível no dmesg
date: 2017-10-20T22:34:17-03:00
author: Sidnei Weber
layout: post
tags: [linux, dmesg]
---
Saída padrão do **dmesg:**

```shell
[ 7515.054630] CPU7: Core temperature above threshold, cpu clock throttled (total events = 29938)
[ 7515.055666] CPU3: Core temperature/speed normal
[ 7515.055668] CPU7: Core temperature/speed normal
[ 9253.929870] CPU4: Core temperature above threshold, cpu clock throttled (total events = 5697)
[ 9253.929873] CPU0: Core temperature above threshold, cpu clock throttled (total events = 5697)
[ 9253.930911] CPU4: Core temperature/speed normal
[ 9253.930913] CPU0: Core temperature/speed normal
```

Saída do comando **dmesg -T:**

```shell
[Sex Out 20 21:56:10 2017] CPU7: Core temperature above threshold, cpu clock throttled (total events = 29938)
[Sex Out 20 21:56:10 2017] CPU3: Core temperature/speed normal
[Sex Out 20 21:56:10 2017] CPU7: Core temperature/speed normal
[Sex Out 20 22:25:08 2017] CPU4: Core temperature above threshold, cpu clock throttled (total events = 5697)
[Sex Out 20 22:25:08 2017] CPU0: Core temperature above threshold, cpu clock throttled (total events = 5697)
[Sex Out 20 22:25:08 2017] CPU4: Core temperature/speed normal
[Sex Out 20 22:25:08 2017] CPU0: Core temperature/speed normal
```

Podemos incluise criar um alias no bashrc:

```shell
alias dmesg="dmesg -T"
```
