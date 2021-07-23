---
title: Reforçando segurança servidor ssh
date: 2016-11-23T10:07:22-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/09/Artigo_07_002-220x162.jpg
tags: [linux, segurança, ssh]
---
### Introdução

Nos dias de hoje é de suma importância proteger nossos servidores contra ataques e invasões. Nesse caso especifico do ssh é importante mantê-lo bem seguro pois é a nossa porta de entrada para o servidor, qualquer brecha pode causar uma catástrofe. Não devemos pensar que por ser um servidor de uma pequena empresa ou um [servidor VPS](https://www.melhorhospedagemdesites.com/servidor-vps/) que não devemos ter esse cuidado, hoje a informação disponível pode estar valendo muito. É uma questão de segurança básica que devemos estar atentos para não termos dores de cabeça e continuar com nossos servidores e serviços intactos.

### Restrições de acesso

Caso queria restringir acesso somente a grupos ou usuários específicos, acrescentaremos ou editaremos as opções abaixo no arquivo de configuração do servidor ssh, o **/etc/ssh/sshd_config**. O mais correto ainda seria criar um grupo somente para acessos ssh.

```shell
AllowGroups sysadmin suporte
AllowUsers pedro juca
```

Podendo usar ou somente restrição por grupo ou somente restrição por usuário.

### Não permitir acesso direto como root

```shell
PermitRootLogin no
```

### Modificar porta padrão

De preferência uma porta com valor alto.

```shell
Port 2258
```

### Bloquear ataques de força bruta com iptables

```shell
iptables -I input -p tcp --dport 22 -i eth0 -m state --state NEW -m recent -set

iptables -I input -p tcp --dport 22 -i eth0 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP

iptables -A INPUT -P tcp --destination-port 22 -j ACCEPT
```

### Logando com chaves de criptografia

Gerar a chave

```shell
ssh-keygen -b 1024 -t dsa
```

![ssh ><](/img/uploads/2016/09/Seleção_009.png)

Copiar para a outra máquina a chave pública

```shell
ssh-copy-id -i ~/.ssh/id_dsa.pub 192.168.1.100
```

![ssh ><](/img/uploads/2016/09/Seleção_010.png)

Verificar se o arquivo foi copiado para o destino em .ssh/authorized_keys e modificar a permissão para 600, por questões de segurança

### Desabilitar autenticação por senha

```shell
PasswordAuthentication no
```
