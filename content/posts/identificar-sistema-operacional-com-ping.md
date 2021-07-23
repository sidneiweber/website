---
id: 712
title: Identificar sistema operacional com ping
date: 2019-01-17T15:21:50-03:00
author: Sidnei Weber
layout: post
#cover: /img/ping-command.png
tags: [rede]

---
O comando ping utiliza o protocolo icmp e é muito útil para alguns testes de rede. O que pouca gente sabe é que durante a resposta do comando ping, uma informação pode nos informar qual o sistema operacional está respondendo. Essa informação é TTL (Time to Live).

Ex:

```shell
ping 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.035 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.041 ms
```

Podemos ver que nesse exemplo o TTL é 64 que corresponde ao Linux. É bom lembrar que cada vez que um pacote passa por um roteador é reduzido um do valor TTL. Caso a resposta seja 64, significa que o pacote veio de um Linux e passou por 2 roteadores.

**Unix = 255**  
**Linux = 64**  
**Windows = 128**
