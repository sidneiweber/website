---
title: Utilizando o Crontab
description: Utilizando o Crontab
date: 2015-07-06T15:45:15-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
---
| Comando | Função |
|---|---|
| crontab -e | Edita o crontab atual do usuário |
| crontab -l | Exibe o atual conteúdo do crontab do usuário |
| crontab -r | Remove o crontab do usuário |


A linha é dividida em 6 campos separados por tabs ou espaço:

| Campo | Função |
|---|---|
| 1° | Minuto |
| 2° | Hora |
| 3° | Dia do mês |
| 4° | Mês |
| 5° | Dia da semana |
| 6° | Programa para execução |

| Campo | Valores |
|---|---|
| Minuto | 0-59 |
| Hora | 0-23 |
| Dia do mês | 1-31 |
| Mês | 1-12 |
| Dia da semana | 0-6 (o “0″ é domingo), 1 é segunda, etc. |
      
[Fonte](http://www.devin.com.br/crontab/)      