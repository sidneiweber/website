---
title: 'Instalar servidor Samba [Dica rápida]'
date: 2014-02-11T23:59:10-03:00
author: Sidnei Weber
layout: post
tags:
  - samba
  - servidores
---
Primeiramente instalaremos o pacote:

```shell
apt-get install samba
```

Depois alteramos a configuração do arquivo:

```shell
vi /etc/samba/smb.conf
```

E alteramos alguns parametros:

```
[global]
#nome do grupo de trabalho
workgroup = casa
#como a maquina irá aparecer na rede Windows
netbios name = servidor de arquivos
#autenticação
#modo de acesso ao servidor
security = user / share
#lembrado que user é quando se criará um usuário no sistema, e share sem usuário

#compartilhamentos
[arquivos]
#descrição do compartilhamento
comment = meus arquivos
#caminho da pasta no servidor
path = /arquivos # ou o diretório desejado
public = yes
browseable = yes #se está visível ou oculto na rede
writeable = yes #permite escrita
read only = no #somente leitura
valid users = VOCE

# define a mascara em que os arquivos serão criados
create mask = 0700 #(terão a permissão rwx somente para o root)

# define a mascara em que os diretórios serão criados
directory mask = 0700
```

Vamos criar o diretório compartilhado no servidor:

```shell
mkdir /arquivos
```

Adicionando um usuário para acessar o Samba:

```shell
smbpasswd -a VOCE
```

Reinicie o serviço:

```shell
service smbd restart
```

Pronto, servidor disponível. [Fonte](http://cristianojosef.blogspot.com.br/2013/02/configurando-um-servidor-samba-no.html)