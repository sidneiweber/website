---
title: Monitorar a performance do MySQL com Mytop
date: 2013-04-23T12:17:04-03:00
author: Sidnei Weber
layout: post
tags:
  - mysql
---
Desenvolvido por _Jeremy D. Zawodny_, o _Mytop_ é uma ferramenta para monitorar o MySQL baseada em console (sem interface gráfica). É utilizada para verificar o desempenho geral e threads do MySQL.

Roda na maioria dos sistemas [Linux](http://www.vivaolinux.com.br/linux/)/Unix (incluindo Mac OS X), que tenham o Perl, DBI e Term:: ReadKey instalados. E com o Term:: ANSIColor instalado, você ainda terá cores. Se você instalar o Time::HiRes, você terá consultas de status em tempo real/segundos.

Plataformas suportadas:

  * Linux
  * FreeBSD
  * Mac OS X
  * BSDI
  * Solaris
  * Windows

Vamos instalar o Mytop, abra o terminal (console) e siga as instruções.

Para sistemas que utilizam o apt-get, você pode instalar como este comando:

```shell
sudo apt-get install mytop
```

Em sistemas baseados no Red Hat, como Fedora, você pode executar o comando:

```shell
yum -y install mytop
```

Se preferir, você pode fazer o download do arquivo em:

  * <http://jeremy.zawodny.com/mysql/mytop/>

Execute estes comandos para descompactar e instalar o Mytop:

```shell
tar -zxvf mytop-<version>.tar.gz
$ cd mytop-<version>
$ perl Makefile.PL
$ make
$ make test
$ sudo make install
```

Pronto, a ferramenta está instalada!

# Executando o Mytop

A maneira mais simples de executar Mytop é executar o comando diretamente na linha de comando. No terminal digite:

mytop -u <usuário> -p <senha> -h <host>_

Por exemplo:

```shell
mytop -u tsarmento -p vol2011 -h 172.16.99.253
```

Alguns outros argumentos:

  * &#8221; ? &#8221; &#8211; Exibe ajuda;
  * &#8221; d &#8221; &#8211; Mostra as conexões a uma determinada base de dados &#8211; Nome da base de dado;
  * &#8221; f &#8221; &#8211; Mostra a consulta completa de uma dado ID de processo (deve ser um processo ativo);
  * &#8221; F &#8220;- Desabilita todos os filtros (host, user, and db);
  * &#8221; h &#8221; &#8211; Mostra apenas as consultas de um determinado host, conectar a um computador remoto;
  * &#8221; I &#8221; &#8211; Mostra o status do InnoDB;
  * &#8221; k &#8221; &#8211; Mata um processo;
  * &#8221; m &#8221; &#8211; Muda o modo de exibição de top para qps (Queries Per Second Mode). Ele exibirá na tela a quantidade de querys por segundo;
  * &#8221; o &#8221; &#8211; Inverte a ordem padrão de ordenação;
  * &#8221; p &#8221; &#8211; Pausa a exibição;
  * &#8221; q &#8221; &#8211; Sair do mytop;
  * &#8221; r &#8221; &#8211; Reset os contadores de status do servidor via comando FLUSH STATUS;
  * &#8221; s &#8221; &#8211; Muda o tempo de atualização do refresh (em segundos);
  * &#8221; u &#8221; &#8211; Mostra os processos de um determinado usuário;
  * &#8221; P &#8221; &#8211; Especifica uma porta não-padrão do MySQL para conectar;

Se você não quer ter que lembrar suas opções, pode criar um arquivo _~/.mytop_ para armazenar os argumentos neste formato:

```
user=root
pass=
host=localhost
db=minhabasededados
delay=5
port=3306
socket=
batchmode=0
header=1
color=1
idle=1
```

Usando um arquivo de configuração irá ajudar a assegurar que a sua senha do banco de dados não fique visível aos usuários na linha de comando. Apenas certifique-se de que as permissões do arquivo ~/.mytop estão de tal forma que os outros usuários não tenham permissão de leitura (a menos que você queira, claro).

Você pode ter algum espaço em branco nas linhas do arquivo de configuração, depois do =. Para mais informações acesse:

  * <http://jeremy.zawodny.com/mysql/mytop>

Agradeço a todos pela atenção.

Viva o Linux, porque nós amamos a Liberdade!

[Fonte](http://www.vivaolinux.com.br/dica/Monitorar-a-performance-do-MySQL-com-Mytop)