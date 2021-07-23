---
layout: post
title: Instalar Ansible AWX com Docker no Centos 7
subtitle: Passo a passo da instalação usando Docker.
description: Ansible AWX é a versão OpenSource do [ansible Tower](https://www.ansible.com/products/tower), produto  comercial desenvolvido pela Red Hat. O AWX fornece uma interface de usuário baseada na Web, API REST e um mecanismo de tarefas construído sobre o Ansible. Neste tutorial, mostrarei como instalar e configurar o AWX usando o Docker.
date: '2019-09-09 16:55:50'
tags:
- ansible
#cover: "/img/ansible.png"
author: Sidnei Weber
---

Ansible AWX é a versão OpenSource do [ansible Tower](https://www.ansible.com/products/tower), produto  comercial desenvolvido pela Red Hat. O AWX fornece uma interface de usuário baseada na Web, API REST e um mecanismo de tarefas construído sobre o Ansible.

Neste tutorial, mostrarei como instalar e configurar o AWX usando o Docker.

Desabilitar SELinux:

```shell
systemctl stop firewalld
systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
```

Habilitar repositório epel:

```bash
yum install -y epel-release
```

Instalar pacotes necessários:

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2 ansible git python-devel python-pip python-docker-py vim-enhanced
```

Configurar repositório stable do Docker e instalá-lo

```shell
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce -y
```

Iniciar serviço

```bash
systemctl start docker
systemctl enable docker
```

Clonar repositório AWX

```bash
git clone https://github.com/ansible/awx.git
cd awx/
git clone https://github.com/ansible/awx-logos.git
pwd
/root/awx
```

Entrar na pasta de instalação

```bash
cd installer/
```

Executar playbook da instalação

```bash
ansible-playbook -i inventory install.yml -vv
```

A instalação irá subir alguns container, pode conferir com o comando abaixo:

```bash
docker container ls
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                                NAMES
318c7c95dcbb        ansible/awx_task:latest   "/tini -- /bin/sh -c."   12 minutes ago      Up 12 minutes       8052/tcp                             awx_task
642c2f272e31        ansible/awx_web:latest    "/tini -- /bin/sh -c."   12 minutes ago      Up 12 minutes       0.0.0.0:80->8052/tcp                 awx_web
641b42ab536f        memcached:alpine          "docker-entrypoint.s."   18 minutes ago      Up 18 minutes       11211/tcp                            memcached
b333012d90ac        rabbitmq:3                "docker-entrypoint.s."   19 minutes ago      Up 19 minutes       4369/tcp, 5671-5672/tcp, 25672/tcp   rabbitmq
ada52935513a        postgres:9.6              "docker-entrypoint.s."   19 minutes ago      Up 19 minutes       5432/tcp                             postgres
[root@awx installer]#
```

Para acessar basta acessar no navegador o endereço do seu computador. Se aparecer a tela login está tudo pronto para usar.
O usuário para login é "admin" e a senha é "password".

![Login AWX](/img/awx.jpg)
