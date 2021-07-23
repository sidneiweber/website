---
title: Monitoramento via TCP no Nagios
description: Monitoramento via TCP no Nagios
date: 2017-04-28T11:06:44-03:00
author: Sidnei Weber
layout: post
cover: /img/uploads/2017/04/Captura-de-tela_2017-04-28_11-05-19.png
tags: [nagios]
---
Hoje vamos dar seguinte a configuração do Nagios [que instalamos um tempo atrás](http://sidneiweber.com.br/instalando-nagios-instalacao-basica/).

Iniciaremos por um dos monitramentos mais simples que é o monitoramento via TCP, caso tennha alguns sites e queira monitorar caso algum deles caia, será muito útil.

Bom a estrutura do Nagios é bem simples mas não mostrarei hoje, para melhorar nos organizarmos vamos criar um arquivo de template próprio, dentro da pasta /usr/local/nagios/etc. Aqui no meu caso crei o arquivo templatesNagios.cfg. Já com o template do servico de monitoramento e templates para monitorar linux, windows e hosts de rede.

```shell
### Template do serviço de monitoramento de Rede e ICMP
define service{
	name TemplateService
	active_checks_enabled 1
	notifications_enabled 1
	passive_checks_enabled 0
	retain_status_information 1
	is_volatile 0
	max_check_attempts 3
	check_interval 3
	normal_check_interval 5
	retry_check_interval 5
	check_period 24x7
	notification_interval 0
	notification_period 24x7
	notification_options u,c,r
	register 0
}

#### Template "HOST" Windows
define host{
	name TemplateHostWindows
	max_check_attempts 3
	check_interval 5
	retry_check_interval 5
	active_checks_enabled 1
	passive_checks_enabled 0
	check_period 24x7
	retain_status_information 1
	notification_interval 60 ; tempo de envio de alerta
	notification_period 24x7
	notification_options d,u,r
	register 0
}

### Template "HOST" Linux
define host{
	name TemplateHostLinux
	check_command check-host-alive
	max_check_attempts 3
	check_interval 3
	retry_check_interval 5
	active_checks_enabled 1
	check_period 24x7
	retain_status_information 1
	notification_interval 60
	notification_period 24x7
	notification_options d,u,r
}

### Template "HOST" Rede
define host{
	name TemplateHostRede
	check_command check-host-alive
	max_check_attempts 2
	check_interval 5
	retry_check_interval 5
	active_checks_enabled 1
	check_period 24x7
	retain_status_information 1
	notification_interval 60
	notification_period 24x7
	notification_options d,u,r
}

# 'check_tcp' command definition
define command{
	command_name check_tcpNP
	command_line $USER1$/check_tcp -H $HOSTADDRESS$ -p $ARG1$ -w $ARG2$ -c $ARG3$
}
```

Feito isso precisamos adicionar nosso arquivo de configuração no arquivo /usr/local/nagios/etc/nagios.cfg

```shell
cfg_file=/usr/local/nagios/etc/templatesNagios.cfg
```

Agora dentro da pasta /usr/local/nagios/etc/ criaremos a pasta network, onde adicionaremos todos os nossos hosts para monitoramento via TCP. Criaremos em outro momento uma pasta linux e uma windows, ficando muito mais organizada nossas configurações. Também precisamos adicionar esse caminho no arquivo /usr/local/nagios/etc/nagios.cfg

```shell
cfg_dir=/usr/local/nagios/etc/network/
```

Dentro da pasta network vamos criar nosso primeiro host para monitorar. Vou criar o arquivo sidneiweber.cfg com o conteúdo abaixo. As opções são auto explicativas, mas comentei para facilitar o entendimento.

```shell
define host{
	host_name Site_Sidnei ; Nome que irá aparecer no nagios
	use TemplateHostRede ; Template a ser usado. Configura no nosso templatesNagios.cfg
	alias Site Sidnei Weber ; Um alias
	address www.sidneiweber.com.br ; O endereço
	contact_groups admins ; Grupo que receberá os alertas

}

define service{
	use TemplateService ; Template do serviço
	host_name Site_Sidnei ; Nome do seu servidor
	service_description PING-Disponibilidade ; Descrição do Serviço a ser monitorado para o host
	check_command check_ping!100,20%!200,60% ; Plugin e Parametros
	contact_groups admins

}

define service{
	use TemplateService
	host_name Site_Sidnei ; Nome do seu servidor
	service_description HTTP ; Descrição do Serviço a ser monitorado para o host
	check_command check_tcpNP!80!1!2! ; Plugin e Parametros
	contact_groups admins
}
```

Após essa configuração basta verificar se está tudo configurado sem erros com o comando:

```shell
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

Se não retornar nenhum erro, basta iniciar o serviço do nagios:

```shell
/usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```

Caso o serviço já esteja rodando e precise reiniciar, eu faço da seguinte maneira:

```shell
# Matando todos os processos nagios
killall nagios
# Iniciando
/usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```

Pronto, primeiro serviço monitorado, e isso pode ser feito com qualquer site ou serviço de ping, uma monitoração simples, mas não deixa de ser um monitoramento.
