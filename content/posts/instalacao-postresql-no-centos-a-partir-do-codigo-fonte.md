---
id: 559
title: Instalação Postresql no Centos a partir do código fonte
date: 2018-05-03T21:28:13-03:00
author: Sidnei Weber
layout: post
#cover: /img/uploads/2018/05/download.png
tags:
  - postgresql
  - centos
---
Estou realizando a instalação na última versão do Centos no momento, que seria a 7. Porém o procedimento para outras versões deve seguir o mesmo padrão.

Instalando dependências:

```shell
yum install -y make wget gcc readline-devel zlib-devel
```

Vamos baixar a última versão do Postresql (10.3 no momento dos testes) e após o download concluído iremos descompactar:

```shell
wget -c https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz
tar -xzvf postgresql-10.3.tar.gz
```

Após isso vamos realizar o processo de compilação, instalação e criação do usuário **postgres**. Dependendo da configuração de sua máquina esse processo pode demorar um pouco:

```shell
./configure
gmake
gmake install
adduser postgres
mkdir -p /usr/local/postgres/data
chown postgres /usr/local/postgres/data
```

Iniciaremos o serviço:

```shell
su - postgres
/usr/local/pgsql/bin/initdb -D /usr/local/postgres/data
/usr/local/pgsql/bin/postgres -D /usr/local/postgres/data >logfile 2>&1 &
```

```shell
[postgres@a984adfc81b5 ~]$ ps -ef |grep postgres
root      8636     1  0 00:21 pts/0    00:00:00 su - postgres
postgres  8637  8636  0 00:21 pts/0    00:00:00 -bash
postgres  8667  8637  0 00:23 pts/0    00:00:00 /usr/local/pgsql/bin/postgres -D /usr/local/postgres/data
postgres  8669  8667  0 00:23 ?        00:00:00 postgres: checkpointer process   
postgres  8670  8667  0 00:23 ?        00:00:00 postgres: writer process   
postgres  8671  8667  0 00:23 ?        00:00:00 postgres: wal writer process   
postgres  8672  8667  0 00:23 ?        00:00:00 postgres: autovacuum launcher process   
postgres  8673  8667  0 00:23 ?        00:00:00 postgres: stats collector process   
postgres  8674  8667  0 00:23 ?        00:00:00 postgres: bgworker: logical replication launcher   
postgres  8677  8637  0 00:23 pts/0    00:00:00 ps -ef
```

Agora com tudo instalado e serviço iniciado realizar alguns testes:

```shell
/usr/local/pgsql/bin/createdb teste
/usr/local/pgsql/bin/psql teste
psql (10.3)
Type "help" for help.

teste=#
```

![](/img/uploads/2018/05/Captura-de-tela-de-2018-05-03-21-27-17.png)

Instalação finalizada com sucesso 🙂
