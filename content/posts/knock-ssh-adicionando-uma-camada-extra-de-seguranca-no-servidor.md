---
layout: post
title: Knock SSH - Adicionando uma camada extra de segurança no servidor
date: '2017-03-21 16:03:09 -0300'
author: Sidnei Weber
cover: "/img/uploads/2017/03/Selecao_008.png"
tags:
- ssh
- segurança
---

Hoje vamos falar do **Knock,** uma ferramenta muito interessante para quem precisar acessar seus servidores remotamente. Bom o que o Knock faz, ele adiciona essa camada a mais da seguinte forma, por exemplo se acessamos nosso servidor pela porta 22 do ssh ela deveria estar liberada. Porém com Knock ela pode estar bloqueada, você acertando uma sequência específica de portas ele irá liberar a porta 22, e somente se acertar a sequencia definida.

### No servidor

Precisamos instalar somente um pacote em nosso servidor:

```shell
apt-get install knockd
```

O arquivo de configuração fica no **_/etc/knockd.conf._** Eis a configuração padrão que veio no Debian.

![ssh ><](/img/uploads/2017/03/Selecao_007.png) 

Eu alterei para a configuração ficar como no exemplo abaixo, sempre colocando a regra de ACCEPT na primeira linha, pois caso tenha sido bloqueada e seja adiciona no final do arquivo a regra não funcionará.

```shell
[options]
        UseSyslog

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -I INPUT 1 -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn
```

Após editaremos o arquivo **_/etc/default/knockd_** para especificar nossa placa de rede:

```shell
START_KNOCKD=1
KNOCKD_OPTS="-i enp0s3"
```

Feito a configuração reiniciaremos o serviço:

```shell
/etc/init.d/knockd restart
```

Caso não tenha a porta 22 fechada, vamos fecha-lá. Lembrando que isso deve estar no script firewall do seu servidor.

```shell
iptables -A INPUT -p tcp --dport 22 -j DROP
```

### No cliente

No cliente instalaremos o mesmo utilitário, porém sem precisar fazer as configurações feitas anteriormente.

```shell
aptitude install knockd
```

Agora vamos bater nas portas em sequência conforme configurado no nosso servidor (7000, 8000, 9000)

```shell
knock 10.0.0.106 7000:tcp 8000:tcp 9000:tcp
```

Com as batidas corretamente executadas, podemos verificar o log do nosso servidor e veremos que o foi passado por 3 estágios e após os mesmos estarem corretos, nosso porta 22 foi liberada pelo firewall

```s
Mar 21 15:49:21 server knockd: starting up, listening on enp0s3
Mar 21 15:49:26 server knockd: 10.0.0.103: openSSH: Stage 1
Mar 21 15:49:26 server knockd: 10.0.0.103: openSSH: Stage 2
Mar 21 15:49:26 server knockd: 10.0.0.103: openSSH: Stage 3
Mar 21 15:49:26 server knockd: 10.0.0.103: openSSH: OPEN SESAME
Mar 21 15:49:26 server knockd: openSSH: running command: /sbin/iptables -I INPUT 1 -s 10.0.0.103 -p tcp --dport 22 -j ACCEPT
```

E assim podemos conectar normalmente sem bloqueio nenhum.

```shell
ssh sidnei@10.0.0.106
sidnei@10.0.0.106's password:
```

### Fechando a porta

Depois de fazer tudo que era necessário podemos fechar a porta novamente, acertando a sequência de fechamento que também é incluida no nosso arquivo de configuração do servidor.

```shell
knock 10.0.0.106 9000:tcp 8000:tcp 7000:tcp
```

E a porta será fechada novamente conforme o log nos informa:

```
Mar 21 15:58:27 server knockd: 10.0.0.103: closeSSH: Stage 1
Mar 21 15:58:27 server knockd: 10.0.0.103: closeSSH: Stage 2
Mar 21 15:58:27 server knockd: 10.0.0.103: closeSSH: Stage 3
Mar 21 15:58:27 server knockd: 10.0.0.103: closeSSH: OPEN SESAME
Mar 21 15:58:27 server knockd: closeSSH: running command: /sbin/iptables -D INPUT -s 10.0.0.103 -p tcp --dport 22 -j ACCEPT
```

E era isso, acredito que seja uma camada bem interessante para nossos servidores e o mais importante, não colocando portas padrões na configuração, será muito mais dificil acertar a sequência para poder liberar o ssh.

[Fonte](https://www.vivaolinux.com.br/artigo/KNOCK-+-SSH?pagina=1)

Um forte abraço
