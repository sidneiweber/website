---
id: 659
title: Convertendo arquivos DOS ^M com Vim
date: 2018-11-06T10:03:27-03:00
author: Sidnei Weber
layout: post
#cover: /img/character-data.png
tags: [linux, shell script]
---
Quem nunca passou pela situação de executar um script e o mesmo apresentar erro. Normal, mas algumas vezes o erro ocorre pela formatação, principalmente se foi escrito ou salvo em um Windows. Ocorre de no final de cada linha ele acrescentar um ^M, o que não o Linux não consegue interpretar.

Existem algumas formas de corrigir esse problema, a mais conhecida é usando o aplicativo [dos2unix](https://sourceforge.net/projects/dos2unix)

Mas o quero apresentar hoje é usando o poderoso **Vim.** Basta estando dentro do arquivo digitar o comando abaixo:

```vim
:set ff=unix
```

Simples e rápido, caso por algum motivo queira fazer o processo contrário, basta executar:

```vim
:set ff=dos
```
