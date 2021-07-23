---
layout: post
title: Pigz - Compactador eficiente e rápido
subtitle: Esse compactador consegue usar múltiplos núcleos.
description: Esse compactador consegue usar múltiplos núcleos.
date: '2019-06-21 14:06:42 -0300'
author: Sidnei Weber
#cover: "/img/pigz.jpg"
comments: true
tags:
- gzip
- pigz
author: Sidnei Weber
---

Talvez nem todos saibam mas a compactação usando gzip temos uma limitação da ferramenta não conseguir executar com múltiplos processadores. Para contornos essa limitação podemos usar uma ferramenta chama [PIGZ](https://zlib.net/pigz/), que usando threads consegue utilizar múltiplos processadores.

A instalação depende da sua distribuição, mas utiliziando as mais comuns temos ela nos repositórios. Caso não tenho basta baixar o [fonte no site deles.](https://zlib.net/pigz/pigz-2.4.tar.gz)

**Manjaro:**

```shell
pacman -S pigz
```

**Ubuntu:**

```shell
apt-get install pigz
```

O uso do pigz se dá assim:

```shell
Usage: pigz [options] [files ...]

tar -c --use-compress-program=pigz -f tar.file dir_to_zip

tar --use-compress-program=pigz -cf OUTPUT_FILE.tar.gz paths_to_archive
```

Para teste vamos compactar uma mesma pasta usando as duas ferramentas, gzip e pigz usando compressão máxima (-9)

```shell
╭─sidnei@black ~
╰─$ du -sh web             
4,2G	web
```

### Usando GZIP

```shell
╭─sidnei@black ~
╰─$ time tar -cf - web/ |gzip -9 - &gt; web.tar.gz
tar -cf - web/  2,81s user 30,31s system 9% cpu 5:56,01 total
gzip -9 - &gt; web.tar.gz  267,42s user 6,43s system 76% cpu 5:56,01 total
╭─sidnei@black ~
╰─$ du -h web.tar.gz
2,2G	web.tar.gz
```

### Usando PIGZ

```shell
╭─sidnei@black ~
╰─$ time tar -c --use-compress-program="pigz -9" -f web.tar.gz web/                                                                                                        130 ↵
tar -c --use-compress-program="pigz -9" -f web.tar.gz web/  356,10s user 35,11s system 175% cpu 3:43,30 total
╭─sidnei@black ~
╰─$ du -sh web.tar.gz  
2,2G	web.tar.gz
```

Na comparação a compactação teve o mesmo tamanho final, porém com o uso de CPU muito maior por parte do PIGZ, lembrando que quanto melhor sua máquina, mais recursos, melhor será o desempenho.

É claro que dependendo a quantidade de arquivos e os tamanhos você não notará tanta diferença, mas com grandes quantidades de arquivos é notório o melhor desempenho. [Nesse site](https://vbtechsupport.com/1576/) temos um comparativo bem legal com várias ferramentas de compressão.
