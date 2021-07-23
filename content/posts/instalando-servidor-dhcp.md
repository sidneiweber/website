---
title: Instalando servidor DHCP
date: 2017-03-06T14:55:51-03:00
author: Sidnei Weber
layout: post
guid: http://sidneiweber.com.br/?p=392
cover: /img/uploads/2017/03/dhcp.png
tags:
  - linux
  - rede
  - dhcp
---
### Definição:

Resumidamente, o DHCP opera da seguinte forma:

  * Um cliente envia um pacote UDP em _broadcast_ (destinado a todas as máquinas) com uma requisição DHCP (para a porta 67);
  * Os servidores DHCP que capturarem este pacote irão responder (se o cliente se enquadrar numa série de critérios — ver abaixo) para a porta 68 do _Host_ solicitante com um pacote com configurações onde constará, pelo menos, um endereço IP, uma máscara de rede e outros dados opcionais, como o gateway, servidores de DNS, etc&#8230;

Fonte: [Wikipedia](https://pt.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)

Esse seria o desenho dos envios de pacotes de um cliente para um servidor DHCP.<figure id="attachment_395" aria-describedby="caption-attachment-395" style="width: 400px" class="wp-caption alignnone">

![dhcp ><](/img/uploads/2017/03/dhcp.jpg) 

### Instalação:

Iremos fazer a instalação do nosso servidor no Debian. Para tal operação iremos instalar o pacote **isc-dhcp-server,** que substituiu o pacote dhcp-server3.

```shell
apt-get install isc-dhcp-server
```

### Configurando:

Primeiramente editaremos o arquivo: **/etc/default/isc-dhcp-server** e colocaremos a placa de rede interna na configuração:

```shell
# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="enp0s8"
```

A configuração básica para o funcionamento é tão simples quanto a instalação. Editaremos o arquivo **/etc/dhcp/dhcpd.conf**. Acrescentaremos as opções básicas para o funcionamento.

```shell
subnet 10.0.0.0 netmask 255.255.255.0 {
        range 10.0.0.100 10.0.0.120;
        option routers 10.0.0.1;
        option domain-name-servers 8.8.8.8;
        option broadcast-address 10.0.0.255;
}
```

Explicando:

**Subnet**: Iniciaremos uma sub-rede para ceder IP's.  
**Range**: A faixa de IP's que será distribuida.  
**Option Routers**: Configura a rota padrão.  
**Option domain-name-servers**: Configura os servidores DNS.**Option broadcast-address**: Indica o fim da sub-rede.

Alguns outros parâmetros básicos que já vem no arquivo por padrão:

**Option domain-name:** domínio.

**Default-lease-time:** Tempo que o servidor verifica se o IP ainda está em uso.

**Max-lease-time:** Tempo máximo de um IP.

Agora é só salvar o arquivo que editamos e reiniciar o serviço:

```shell
/etc/init.d/isc-dhcp-server restart
```

Pronto teremos um servidor DHCP dando IP para a nossa rede. Ainda temos algumas opções criar duas redes distintas dentro do mesmo servidor, atrelar IP's ao Mac Address, negar máquinas que não estejam cadastradas no servidor DHCP, enfim, inúmeros recursos que estudaremos mais adiante.

Obrigado e até a próxima.