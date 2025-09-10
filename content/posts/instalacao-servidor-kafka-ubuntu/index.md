---
layout: post
title: Instalação servidor Kafka no Ubuntu Server
description: '
Kafka é um dos serviços mais utilizados quando estamos falando de streaming de eventos e mensageria. Nesse artigo vamos realizar a instalação de um servidor Kafka utilizando Ubuntu Server e também realizaremos algumas operações para enviar e receber mensagens no servidor.
'
date: '2024-01-10 10:20:00'
tags: [kafka, zookeeper]
permalink: /instalacao-servidor-kafka-ubuntu-server/
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
author: Sidnei Weber
---
<!--more-->

## O que é o Kafka

Resumidamente o Kafka é usado para trabalhar com fila de mensagens e como uma plataforma de streaming de eventos, usando um modelo de "publicar/assinar". Foi criado e disponibilizado pelo [Linkedin](https://github.com/linkedin/kafka) em 2011. Ele permite que os produtores consigam gravar mensagens no Kafka, que posteriormente podem ser lidas por um ou mais consumidores. Esses registros não podem ser modificados após serem enviados para o Kafka.
Ele é executado como um cluster de um ou mais servidores, ou seja, mesmo que só tenhamos um servidor ele mesmo assim é considerado um cluster. Cada nó desse cluster é também chamado de broker.

[Para saber mais detalhes sobre Kafka acesse meu outro post.](https://sidneiweber.com.br/kafka-tudo-que-precisamos-saber/)

## Instalação do Java

Neste tutorial, usaremos o Ubuntu 22.04 LTS Server, mas as etapas também devem ser semelhantes em outras versões do Ubuntu.
Antes de começar, vamos nos certificar que o Java está instalado no servidor. Você pode verificar se o Java já está instalado executando o seguinte comando:

```shell
java -version
Command 'java' not found, but can be installed with:
sudo apt install openjdk-11-jre-headless  # version 11.0.20.1+1-0ubuntu1~22.04, or
sudo apt install default-jre              # version 2:1.11-72build2
sudo apt install openjdk-17-jre-headless  # version 17.0.8.1+1~us1-0ubuntu1~22.04
sudo apt install openjdk-18-jre-headless  # version 18.0.2+9-2~22.04
sudo apt install openjdk-19-jre-headless  # version 19.0.2+7-0ubuntu3~22.04
sudo apt install openjdk-8-jre-headless   # version 8u382-ga-1~22.04.1

```

Se o Java não estiver instalado, como no exemplo acima, você poderá instalá-lo executando o seguinte comando:

```shell
sudo apt-get update && sudo apt-get install default-jre
```

## Instalação do Kafka

Agora que temos o Java instalado, podemos prosseguir com a instalação do Kafka. Primeiro, baixe a versão mais recente do Kafka no site do [Apache Kafka](https://kafka.apache.org/downloads). No momento que estou escrevendo, a versão mais recente é a 3.6.1. Você pode baixá-lo usando wget:

```shell
wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
```

Após baixar o arquivo, vamos extrai-lo:

```shell
tar -xzf kafka_2.13-3.6.1.tgz
```

Isto criará um novo diretório chamado "kafka_2.13-3.6.1". Entre para este diretório:

```shell
cd kafka_2.13-3.6.1
```

O Kafka usa um arquivo de configuração chamado `server.properties`, a configuração padrão deve funcionar para a maioria dos casos de uso, mas você pode personalizá-lo conforme necessário. Você pode, por exemplo, alterar o número da porta que o Kafka executa ou o número de threads do servidor, para mais detalhes podemos acessar a [documentação](https://kafka.apache.org/documentation/#configuration). Se quiser dar uma conferida no arquivo, podemos executar o comando abaixo:

```
cat config/server.properties
```

## Iniciar o servidor Kafka

"Aviso do autor" 
Gostaria de deixar claro que as instruções a seguir não se aplicam a um ambiente de produção, eu considero esse cenário ideal para ambientes de testes ou cenários específicos como testar alguma configuração ou algo do gênero.


Aqui teremos duas opções para iniciar o servidor Kafka, o modo tradicional, iniciando o servidor Kafka com o Zookeeper e o modo "Kraft" que é uma maneira recente de iniciar o Kafka sem utilizar o Zookeeper, saiba mais sobre [aqui](https://developer.confluent.io/learn/kraft/). Escolha uma das opções para iniciar o servidor e então poderemos avançar para os testes.

### Iniciando servidor com Zookeeper

Em um terminal iremos iniciar o Zookeeper, lembrando que os comandos precisam ser executados nessa ordem:

```shell
bin/zookeeper-server-start.sh config/zookeeper.properties
```

Em outro terminal vamos iniciar o servidor Kafka:

```shell
bin/kafka-server-start.sh config/server.properties
```

### Iniciando servidor sem Zookeeper

Para iniciar o Kafka sem Zookeeper executaremos os comandos abaixo, primeiro geraremos um valor de Cluster UUID:

```shell
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```

Com o valor de Cluster UUID iremos formatar o diretório de logs:

```shell
bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
```

E por fim iniciaremos o servidor Kafka:

```shell
bin/kafka-server-start.sh config/kraft/server.properties
```

## Realizando alguns testes

Alguns exemplos de eventos que podem ser usados em um servidor Kafka são transações de pagamento, geolocalização, pedidos de compras, medições de sensores de dispositivos IoT e diversas outras. Esses eventos são organizados em tópicos. De maneira simples o tópico é como se fosse uma pasta, uma maneira de organizar as coisas.

Agora que o servidor Kafka está em execução (podemos ver pelas mensagens de log quando iniciamos o servidor), podemos realizar alguns testes para ver o servidor em funcionamento. Para nosso teste vamos criar um tópico e enviar alguma mensagem para ele. Para criar o tópico vamos executar o seguinte comando:

### Criando Tópicos

```shell
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic teste
Created topic teste.
```

Para validar se o tópico foi criado, podemos listar os tópicos do servidor:

```shell
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
Created topic teste.
```

Também podemos descrever o tópico criado para entender os detalhes:

```shell
bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic teste
Topic: teste	TopicId: _oSFFE0hTTy3I7t1qUDwEQ	PartitionCount: 1	ReplicationFactor: 1	Configs: segment.bytes=1073741824
	Topic: teste	Partition: 0	Leader: 1	Replicas: 1	Isr: 1

```

### Produzindo mensagens

Podemos produzir mensagens pela linha de comando, para isso podemos utilizar o producer linha. Cada linha digitada realizará a gravação de um evento separado no tópico.

```shell
bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic teste
>Primeira mensagem
>Segunda mensagem
```

Para encerrar o envio de mensagens aperte as teclas `CTRL+C`

### Consumindo mensagens

Para consumir as mensagens vamos executar o comando abaixo:

```shell
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic teste --from-beginning
Primeira mensagem
Segunda mensagem
```

Se você ver as mensagens digitadas anteriormente é sinal que tudo deu certo. Um teste bem legal de ser feito é deixar o terminal aberta conectado com o consumidor e em outro terminal utilizar o comando para produzir mensagens. Você poderá ver as mensagens entrando imediatamente no terminal onde o consumidor está ativo.

![Consumer Kafka recebendo mensagens](consumer.gif "Consumer trabalhando")

Bom agora já conseguimos fazer o básico no nosso servidor Kafka, em breve estarei escrevendo algumas outras dicas sobre Kafka. Até lá.
