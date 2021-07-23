---
title: Servidor Proxy com squid
date: 2014-02-16T18:53:08-03:00
author: Sidnei Weber
layout: post
tags:
  - servidores
  - squid
---
Instalação:

```shell
apt-get install squid
```

Arquivo base de configuração (/etc/squid/squid.conf):

```
http_port 3128
visible_hostname KORZOS 

acl all src 0.0.0.0/0.0.0.0
acl manager proto cache_object
acl localhost src 127.0.0.1/255.255.255.255
acl SSL_ports port 443 563
acl Safe_ports port 21 80 443 563 70 210 280 488 59 777 901 1025-65535
acl purge method PURGE
acl CONNECT method CONNECT 

http_access allow manager localhost
http_access deny manager
http_access allow purge localhost
http_access deny purge
http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports 

acl redelocal src 192.168.0.0/24 

http_access allow localhost
http_access allow redelocal
http_access deny all

# Define o caminho das páginas de erro do squid.
error_directory /usr/share/squid/errors/Portuguese

# Define o e-mail que vai aparecer na página de erro do Squid, assim o usuário terá mais informações para interagir com o responsável.
cache_mgr admin@seu_dominio.com.br

# Esta ACL é responsável por não armazenar conteúdo CGI em cache.
acl QUERY urlpath_regex cgi-bin ?
no_cache deny QUERY
# Define a quantidade de memória RAM reservada para o uso do Squid.
cache_mem 64 MB 

# Esta linha é responsável por limitar o tamanho dos arquivos que serão armazenados no cache da memória RAM.
maximum_object_size_in_memory 64 KB 

# Aqui definimos o tamanho máximo e mínimo respectivamente dos arquivos que serão armazenados no cache do HD.
maximum_object_size 512 MB
minimum_object_size 0 KB 

# Com essas duas linhas podemos definir a porcentagem de atualização do cache, estamos dizendo que quando o cache chegar em 95% o Squid irá apagar os arquivos mais antigos até chegar a 90%.
cache_swap_low 90
cache_swap_high 95 

# Nessa linha conseguimos definir o tamanho e alguns parâmetros do cache feito em HD, a linha é composta por quatro valores, o 1º define o caminho do cache (/var/spool/squid), o 2º o tamanho que será alocado em MB para o cache (2Gb), o 3º a quantidade de diretórios criados para o cache (16) e o 4º é o numero de subdiretórios que serão criados. Se você possuir bastante espaço em disco e quiser armazenar os arquivos por mais tempo, aumente a opção do tamanha do cache.
cache_dir ufs /var/spool/squid 2048 16 256 

# Define onde serão armazenados os registros de log do Squid.
cache_access_log /var/log/squid/access.log 

refresh_pattern ^ftp: 15 20% 2280
refresh_pattern ^gopher: 15 0% 2280
refresh_pattern . 15 20% 2280
```

Reiniciar squid:

```shell
/etc/init.d/squid restart
```

### Entendendo configuração:
**http_port 3128**: Define em qual porta o Squid vai atuar, a porta default é a 3128, mas podemos definir qualquer outra porta.  
**visible_hostname SERVIDOR**: Define o nome do servidor, lembre-se de substituir o &#8220;KORZOS&#8221; pelo nome do seu servidor.  
**acl all src 0.0.0.0/0.0.0.0**: Esta linha cria uma ACL, uma política de acesso com nome &#8220;all&#8221; contendo qualquer IP.  
**acl localhost src 127.0.0.1/255.255.255.255**: Aqui criamos uma ACL de nome &#8220;localhost&#8221; contendo localhost.  
**acl SSL_ports port 443 563**: Cria a ACL contendo as portas que são utilizadas no protocolo HTTPS.  
**acl Safe_ports port 21 80 443 563 70 210 280 488 59 777 901 1025-65535**: Cria a ACL contendo as portas de diversos protocolos conhecidos na Internet.  
**acl manager proto cache_object**: Cria a ACL manager do tipo proto.  
**acl purge method PURGE** : Cria a ACL manager do tipo method.  
**acl CONNECT method CONNECT**: Cria a ACL CONNECT também do tipo method.  
**http_access allow manager localhost**: Libera a ACL manager e localhost.  
**http_access deny manager** : Bloqueia a ACL manager.  
**http_access allow purge localhost**: Libera a ACL purge e localhost  
**http_access deny purge**: Bloqueia a ACL purge.  
**http\_access deny !Safe\_ports**: Esta linha se torna bastante interessante pelo uso da &#8220;!&#8221;, pois ela bloqueia qualquer conexão que não contenha o conteúdo da ACL Safe_Ports.  
**http\_access deny CONNECT !SSL\_ports**: Bloqueia qualquer conexão que não esteja no conteúdo da ACL SSL_ports.  
**acl redelocal src 192.168.0.0/24**: Cria a ACL redelocal contendo a faixa de endereço da rede.  
**http_access allow localhost**: Libera a ACL localhost.  
**http_access allow redelocal**: Libera a ACL redelocal.  
**http_access deny all**: Bloqueia a ACL all

Criando ACLs  
Arquivos:  
Neste arquivo, iremos adicionar palavras que serão bloqueadas, como: sexo, porno...

```shell
pico /etc/squid/palavras_bloqueadas.txt
```

Neste arquivo, serão adicionados os sites que não terão acesso, como: 4shared.com, rapidshare.com, megavideo.com, filesonic.com, etc:

```shell
pico /etc/squid/sites_bloqueados.txt
```

Aqui, iremos colocar as redes sociais, como: facebook.com, orkut.com, twiter.com, etc:

```shell
pico /etc/squid/redes_sociais.txt
```

Neste arquivo, iremos colocar os IPs das máquinas dos gerentes (e &#8220;daquela&#8221; estagiária que entrou semana passada&#8230;:)):

```shell
pico /etc/squid/ips_liberados.txt
```

Lista de sites adultos: redtub, xvideos

```shell
pico /etc/squid/sites_porno.txt
```

Este arquivo limita os tipos de arquivos que serão baixados, tudo que contiver neste arquivo será bloqueado. Exemplos: .avi$, .mp3$, .wmv$:

```shell
pico /etc/squid/formato_arquivo.txt
```

Adicionar ACLs:

```shell
acl rede_local src 192.168.0.0/24
acl palavras_bloqueadas url_regex -i "/etc/squid/palavras_bloqueadas.txt "
acl sites_bloqueados url_regex -i "/etc/squid/ sites_bloqueados.txt "
acl redes_sociais url_regex -i "/etc/squid/redes_sociais.txt"
acl liberados src "/etc/squid/ips_liberados.txt "
acl porno url_regex -i "/etc/squid/sites_porno.txt "e acl formato_arquivo url_regex -i "/etc/squid/formato_arquivo.txt"
acl horario_almoco time 12:00-13:00 

http_access allow liberados
http_access allow redes_sociais horario_almoco
http_access deny redes_sociais
http_access deny sites_bloqueados
http_access deny palavras_bloqueadas
http_access deny porno
http_access deny formato_arquivo
http_access allow redelocal
```

Criar arquivos de swap

```shell
squid -z
```

E iniciar os serviços

[Fonte](http://www.vivaolinux.com.br/artigo/Servidor-proxy-com-Squid-Instalacao-e-configuracao/?pagina=1)