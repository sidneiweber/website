---
id: 801
title: Instalando MongoDB Community Edition 4.0 no Ubuntu
subtitle: Super fácil instalar o MongoDB no ubuntu usando gerenciador de pacotes.
description: Super fácil instalar o MongoDB no ubuntu usando gerenciador de pacotes.
date: 2019-03-08T16:12:05-03:00
author: Sidnei Weber
layout: post
#cover: /img/uploads/2019/03/mongodb-logo-rgb-j6w271g1xn-768x203.jpg
tags:
    - mongo
---

### <span id="O_que_e_MongoDB">O que é MongoDB</span>

O MongoDB é um banco de dados NoSQL orientado a documentos de alto desempenho (sistema noSQL significa que ele não fornece tabelas, linhas, etc.). Ele armazena dados em documentos semelhantes a JSON com esquemas dinâmicos para melhor desempenho.

### <span id="Adicionando_repositorios">Adicionando repositórios</span>

Para instalar **MongoDB Community Edition** no Ubuntu, precisamos primeiro importar a chave pública usada pelo gerenciador de pacotes.

```shell
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

No Ubuntu 18.04

```shell
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt-get update
```

No Ubuntu 16.04

```shell
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt-get update
```

No Ubuntu 14.04

```shell
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt-get update
```

### <span id="Instalando_o_pacote">Instalando o pacote</span>

```shell
sudo apt-get install -y mongodb-org
```

### <span id="Habilitando_e_iniciando_servico">Habilitando e iniciando serviço</span>

```shell
systemctl enable mongod.service
systemctl start mongod.service
```

E pronto já podemos usar no mongo, lembrando que é necessário liberar a porta 27017 no firewall caso o mesmo esteja habilitado. Nos próximos posts falaremos um pouco mais sobre o mongo.
