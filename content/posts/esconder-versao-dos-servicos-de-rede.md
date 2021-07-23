---
title: Esconder versão dos serviços de rede
date: 2009-09-29T12:16:09-03:00
author: Sidnei Weber
layout: post
tags:
  - rede
  - servidores
---
Escondendo a versão dos serviços que estão rodando em seu servidor para aumentar a segurança Pessoal.

Importante para a segurança de um servidor é esconder a versão de seus serviços para que o invasor não fique procurando uma falha de segurança para aquela versão específica do daemon.

Então vamos ocultar alguns serviços mais utilizados:

**Proftpd**  
=======  
Basta adicionar a seguinte linha no proftpd.conf:

> ServerIdent on ""

A linha acima informa a versão do serviço como sendo "", isso quer dizer que será em branco.

Para validar é necessário reiniciar o Proftpd.

**SSH**  
===  
Já esse é necessário baixar o fonte do site oficial www.openssh.com/ e compilar : D

Após descompactar, terá um arquivo chamado version.h. Altere o conteúdo da seguinte linha:

> #define SSH_VERSION ""

Depois é só compilar normalmente com ./configure, make e make install.

Obs.: O ssh escuta por padrão na porta 22. É altamente recomendável alterar essa porta para qualquer outra porta.

**Apache**  
======  
O Apache, assim como o Proftpd, basta apenas acrescentar uma opção em seu conf conforme a linha baixo:

ServerTokens Prod  
=================

Necessário reiniciar o serviço também.

Para testar se as versões dos daemons estão ocultadas, use o nmap para verificar conforme o exemplo:

```shell
nmap localhost -sV

Starting nmap 3.75 ( http://www.insecure.org/nmap/ ) at 2008-03-05 09:46 BRT  
Interesting ports on localhost (127.0.0.1):  
(The 1658 ports scanned but not shown below are in state: closed)  
PORT STATE SERVICE VERSION  
21/tcp open ftp?  
8080/tcp open http Apache httpd  
Nmap run completed - 1 IP address (1 host up) scanned in 100.332 seconds
```

Porque o ssh não apareceu quando utilizados o nmap para varrer as portas da máquina?

Porque foi alterada para uma porta que não está na lista que o nmap utiliza que está em /usr/share/nmap/nmap-services.

Alguns outros programas identificam o tipo de serviço em qualquer porta que seja, mas não saberá a versão do serviço. Um outro programa que pode ser usado é o Nessus para esse tipo de verificação.