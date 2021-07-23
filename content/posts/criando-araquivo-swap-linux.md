---
layout: post
title: Criando arquivo de SWAP no Linux
description: '
SWAP é um espaço no disco que é utilizado quando a quantidade de memória RAM física está cheia. Quando um sistema Linux fica sem RAM, os blocos de memória inativos são movidos da RAM para a área de SWAP.
'
date: '2021-01-29 22:00:00'
tags: [linux]
author: Sidnei Weber
---

## O que é o SWAP

SWAP é um espaço no disco que é utilizado quando a quantidade de memória RAM física está cheia. Quando um sistema Linux fica sem RAM, os blocos de memória inativos são movidos da RAM para a área de SWAP.

## Instalação

Criando arquivo de SWAP

```bash
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
```
Com o comando acima iremos criar um arquivo de SWAP com 1 GB de tamanho

Iremos ajustar as permissões para o correto funcionamento

```bash
sudo chmod 600 /swapfile
```

Usaremos o comando **mkswap** para setar o arquivo criado anteriormente como uma área de SWAP:

```bash
sudo mkswap /swapfile
```

Habilitaremos o SWAP com o seguinte momento:

```bash
sudo swapon /swapfile
```

Para que essas alterações sejam permanentes precisamos adicionar uma entrada no arquivo **/etc/fstab**:

```bash
/swapfile swap swap defaults 0 0
```

Para verificar se o SWAP está ativo podemos usar os comandos abaixos:

```bash
sudo swapon --show
sudo free -h
```