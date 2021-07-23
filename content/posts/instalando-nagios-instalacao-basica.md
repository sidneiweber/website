---
layout: post
title: Instalando Nagios - Instalação básica
date: '2017-01-31 13:11:40 -0300'
author: Sidnei Weber
#cover: "/uploads/2017/01/nagios_logo_black.png"
tags: [nagios]
---

Vamos fazer a instalação básica do Nagios. Pra quem não conhece o Nagios, segue um [link para conhecer melhor](https://pt.wikipedia.org/wiki/Nagios).

> **Nagios** é uma popular aplicação de monitoramento de rede de [código aberto](https://pt.wikipedia.org/wiki/C%C3%B3digo_aberto "Código aberto") distribuída sob a licença [GPL](https://pt.wikipedia.org/wiki/GNU_General_Public_License "GNU General Public License"). Ele pode monitorar tanto hosts quanto serviços, alertando quando ocorrerem problemas e também quando os problemas são resolvidos.

Faremos a instalação no Debian, que é uma distribuição de minha preferência. O Nagios pode ser instalado em qualquer sistema Linux, a única diferença que pode ocorrer é a instalação de dependências e/ou alguma localização de pastas

### Instalação das dependências

```shell
sudo apt-get install wget build-essential apache2 php-gd libgdchart-gd2-xpm libgdchart-gd2-xpm-dev libapache2-mod-php
```

### Baixando Nagios

Baixaremos a última versão dos pacotes Nagios Core e Nagios Core Plugins pelo site https://www.nagios.org/downloads/. Ou diretamente pelos links abaixo:

[Nagios Core](https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.2.4.tar.gz#_ga=1.92809877.98229964.1480095349)

[Nagios Plugins](https://nagios-plugins.org/download/nagios-plugins-2.1.4.tar.gz#_ga=1.114959247.98229964.1480095349)

### Extraindo Pacotes

```shell
tar -xzvf nagios-4.2.4.tar.gz
tar -xzvf nagios-plugins-2.1.4.tar.gz
```

### Adicionar usuário Nagios

```shell
useradd nagios
groupadd nagios
usermod -a -G nagios nagios
usermod -a -G nagios www-data
```

### Instalação

#### Compilando Nagios

```shell
cd nagios-4.2.4
./configure --with-command-group=nagios

make all
make install
make install-init
make install-config
make install-commandmode
make install-webconf
```

Reiniciando o apache

```shell
service apache2 restart
```

#### Criando usuário e senha do Nagios

```shell
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

#### Compilando os plugins

```shell
cd ..
cd nagios-plugins-2.1.4
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
make install
```

Habilitar CGI no apache

```shell
cp /etc/apache2/mods-available/cgi.load /etc/apache2/mods-enabled/
service apache2 reload
```

Antes de fazer qualquer alteração nas configurações, teste se está tudo ok :

```shell
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

![nagios ><](/img/uploads/2017/01/Selecao_006.png) 

Caso não ocorra nenhum erro, podemos iniciar o serviço:

```shell
/usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```

Basta acessar pelo endereço IP/nagios (ip da máquina na qual foi instalado ou localhost se for local), com usuário nagiosadmin e a senha criada anteriormente. Segue um print do meu nagios com alguns hosts sendo monitorados e com uma interface diferente. Falaremos desses detalhes em outros posts.

![nagios ><](/img/uploads/2017/01/Selecao_005.png)
