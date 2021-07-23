---
id: 821
title: Exemplos de uso do comando ping
subtitle: Alguns usos pouco conhecidos do comando ping.
description: Alguns usos pouco conhecidos do comando ping.
date: 2019-04-26T09:56:31-03:00
author: Sidnei Weber
layout: post
#cover: /img/ping-command.png
tags: [linux, rede]
---
Segundo o Wikipédia ping é …

“**Ping** ou **latência** como podemos chamar, é um utilitário que usa o protocolo ICMP para testar a conectividade entre equipamentos. É um comando disponível praticamente em todos os sistemas operacionais. Seu funcionamento consiste no envio de pacotes para o equipamento de destino e na escuta das respostas. Se o equipamento de destino estiver ativo, uma resposta "pong", uma analogia ao famoso jogo de ping-pong) é devolvida ao computador solicitante. ”

Sabendo dessa teoria agora vamos a alguns exemplos:

**Exemplo 1**: Aumentar ou diminuir o intervalo de tempo entre os pacotes enviados. O ping abaixo esperará cinco segundos antes de enviar o próximo pacote:

```shell
ping -i 5 192.168.3.1
```

E para diminuir o intervalo de tempo:

```shell
ping -i 0.3 192.168.3.1
```

**Exemplo 2**: Verificar se a interface está ativa. Por exemplo:

```shell
ping 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.046 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.054 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.055 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.052 ms
```

Quando obtiver um tempo de resposta é por que a interface está comunicando, caso contrário irá exibir alguma mensagem de erro:

```shell
ping 192.168.3.50
PING 192.168.3.50 (192.168.3.50) 56(84) bytes of data.
From 192.168.3.18 icmp_seq=1 Destination Host Unreachable
From 192.168.3.18 icmp_seq=2 Destination Host Unreachable
From 192.168.3.18 icmp_seq=3 Destination Host Unreachable
From 192.168.3.18 icmp_seq=4 Destination Host Unreachable
```

**Exemplo 3**: Envie N pacotes e pare. No Linux e outras espécies Unix, o comando ping não termina até que você pressione Ctrl + C, para enviar um certo número de pacotes usamos o argumento -c. Vamos testar enviando 2 pacotes:

```shell
ping -c 2 192.168.3.1
PING 192.168.3.1 (192.168.3.1) 56(84) bytes of data.
64 bytes from 192.168.3.1: icmp_seq=1 ttl=30 time=2.72 ms
64 bytes from 192.168.3.1: icmp_seq=2 ttl=30 time=2.70 ms

--- 192.168.3.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3ms
rtt min/avg/max/mdev = 2.696/2.707/2.718/0.011 ms
```

**Exemplo 4**: ping com áudio, envia um beep a cada ping com sucesso

```shell
ping -a localhost
```

**Exemplo 5:** “Inundar” a rede. Dísponivel somente para superusuários, envia cem ou mais pacotes por segundo, imprimindo um ponto por cada pacote enviado e um espaço quando recebido.

```shell
sudo ping -f 192.168.3.1                                                                                                                              2 ↵
[sudo] senha para sidnei:
PING 192.168.3.1 (192.168.3.1) 56(84) bytes of data.
.^C
--- 192.168.3.1 ping statistics ---
1858 packets transmitted, 1857 received, 0.0538213% packet loss, time 412ms
rtt min/avg/max/mdev = 1.962/2.381/48.167/2.565 ms, pipe 3, ipg/ewma 2.373/2.276 ms
```

**Exemplo 6**: Encontrar o endereço IP de um domínio. Quando um ping é disparado em um nome domínio, antes do envio dos pacotes o comando escreve a saída padrão entre parênteses, depois do nome de domínio, o IP do mesmo.

```shell
ping -c1 google.com
PING google.com(2800:3f0:4001:803::200e (2800:3f0:4001:803::200e)) 56 data bytes
64 bytes from 2800:3f0:4001:803::200e (2800:3f0:4001:803::200e): icmp_seq=1 ttl=57 time=33.1 ms

--- google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 33.078/33.078/33.078/0.000 ms
```

**Exemplo 7**: Mostrar apenas as estatísticas do comando. No final do comando é mostrado estatísticas como quantidade de pacotes transmitidos, recebidos, porcentagem de pacotes perdidos e tempo. Se queremos ver somente as estatísticas sem observar cada linha de pacote enviado podemos utilizar a opção -q (quiet).

```shell
ping -c5 -q 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.

--- 127.0.0.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 63ms
rtt min/avg/max/mdev = 0.047/0.058/0.090/0.017 ms
```


**Exemplo 8**: Modificar o tamanho do pacote. Por padrão o tamanho do pacote do ping fica entre 56 a 100 bytes. Se utiliza o tamanho do pacote 100, verá 128 bytes, isto se deve a que 28 bytes é o tamanho do cabeçalho do ping.

```shell
ping -c5 -s 100 localhost
PING localhost(localhost (::1)) 100 data bytes
108 bytes from localhost (::1): icmp_seq=1 ttl=64 time=0.029 ms
108 bytes from localhost (::1): icmp_seq=2 ttl=64 time=0.048 ms
108 bytes from localhost (::1): icmp_seq=3 ttl=64 time=0.047 ms
108 bytes from localhost (::1): icmp_seq=4 ttl=64 time=0.047 ms
108 bytes from localhost (::1): icmp_seq=5 ttl=64 time=0.047 ms

--- localhost ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 73ms
rtt min/avg/max/mdev = 0.029/0.043/0.048/0.010 ms
```

**Exemplo 9**: Especificar o tempo. O parâmetro **-w** especifica o tempo limite para terminar o ping. Por exemplo -w 5, o comando ping terminará após os cinco segundos, independentemente quantos pacotes foram enviados e recebidos.

```shell
ping -w 5 localhost
PING localhost(localhost (::1)) 56 data bytes
64 bytes from localhost (::1): icmp_seq=1 ttl=64 time=0.028 ms
64 bytes from localhost (::1): icmp_seq=2 ttl=64 time=0.046 ms
64 bytes from localhost (::1): icmp_seq=3 ttl=64 time=0.047 ms
64 bytes from localhost (::1): icmp_seq=4 ttl=64 time=0.038 ms
64 bytes from localhost (::1): icmp_seq=5 ttl=64 time=0.081 ms

--- localhost ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 54ms
rtt min/avg/max/mdev = 0.028/0.048/0.081/0.017 ms
```

**Exemplo 10**: Ping online. Existem páginas como [Ping.eu](https://ping.eu/) que nos permitem realizar um ping de diferentes localizações para nosso servidor.

**Exemplo 11**: Estatísticas parciais sem sair. Ao invez de apertar Ctrl+C para terminar (SIGQUIT) o comando ping, podemos utilizar Ctrl+I para mostrar estatísticas parciais e continuar o envio de pacotes.
