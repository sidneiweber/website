---
title: Criar script inicialização Nagios no Debian
date: 2017-02-08T14:17:09-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2017/01/nagios_logo_black.png
tags: [nagios]
---
Após a instalação e inicialização do Nagios no nosso servidor Debian, temos que configurar um script de inicialização, para que cada vez que a gente precisa reiniciar o serviço ou a própria máquina não precisaremos subir tudo na mão.

A primeira coisa é deletar o script já existente, lembrando que esse tutorial é válido para o Debian.

```shell
rm -rf /etc/init.d/nagios
```

Copiar um esqueleto do sistema:

```shell
cp /etc/init.d/skeleton /etc/init.d/nagios
```

Vamos editar o arquivo /etc/init.d/nagios e colocar o seguinte conteúdo. Lembrando de remover ou comentar as duas linhas existentes no final do arquivo.

```shell
# DESC="Description of the service"
# DAEMON=/usr/sbin/daemonexecutablename

DESC="Nagios"
NAME=nagios
DAEMON=/usr/local/nagios/bin/$NAME
DAEMON_ARGS="-d /usr/local/nagios/etc/nagios.cfg"
PROFILE=/usr/local/nagios/var/$NAME.lock
```

Dar as permissões para execução:

```shell
chmod 755 nagios
```

E inicializar o serviço para subir junto com o sistema, quando o servidor for ligado:

```shell
update-rc.d nagios defaults
```

Agora temos todas as opções disponíveis:

```shell
/etc/init.d/nagios
Usage: /etc/init.d/nagios {start|stop|status|restart|try-restart|force-reload}
```

Forte abraço e até a próxima.