---
layout: post
title: Instalando última versão do Ansible no Ubuntu/Debian
description: '
Instalando última versão do Ansible no Ubuntu/Debian.
'
date: '2019-01-22 22:20:00'
#cover: "/img/ansible_ubuntu.png"
tags: [ansible]
author: Sidnei Weber
---
Caso não saiba o que é Ansible, pode dar uma lida [nesse post.](https://sidneiweber.com.br/2018/06/19/ansible-o-que-e-e-para-que-serve/)

A forma mais simples para instalar o Ansible é com o comando abaixo, porém é instalado uma versão mais antiga.

```shell
sudo apt-get update
sudo apt-get install ansible
```

Agora para baixar uma versão mais atualizada basta seguir os passos abaixo:

```shell
sudo aptitude update
sudo aptitude install python-pip
sudo pip install ansible
```
