---
layout: post
title: 4 Alternativas ao Redis
description: '
O Redis é um banco de dados em memória de código aberto, conhecido por seu alto desempenho e versatilidade. Entretanto um polêmica foi criada recentemente com a alteração do tipo de licença, deixando de ser um software livre. Diversas empresas já estão correndo para criar ou buscar alternativas que sejam mais atrativas e com certeza olhem para a comunidade.
'
date: '2024-04-02 12:20:00'
tags: [redis, database]
permalink: /4-alternativas-ao-redis/
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
author: Sidnei Weber
---
<!--more-->

# Mas o que é esse tal de Redis? 

O Redis é um banco de dados em memória de código aberto, conhecido por seu alto desempenho e versatilidade. Utilizado principalmente como um armazenamento em cache, ele também suporta estruturas de dados complexas, como listas, conjuntos e mapas, o que o torna ideal para aplicativos que necessitam de baixa latência e alta escalabilidade. Além disso, sua capacidade de persistência e replicação o tornam uma opção viável para casos de uso que exigem alta disponibilidade e confiabilidade nos dados.

Porém, uma polêmica foi lançada recentemente, a empresa por trás do Redis decidiu mudar a [licença de distribuição](https://github.com/redis/redis/pull/13157), e com essa nova licença o projeto passará a não ter mais uma licença que se enquadre como software livre. Caso queira mais detalhes sobre o caso, recomendo assistir o vídeo abaixo que contém bastante informação:

{{< youtube S3LtOQDxCJk >}}

# As principais alternativas

## Valkey

O projeto mais promissor até o momento, pois o projeto é basicamente uma continuação do Redis mantendo as licenças no modelo opensource. O nome é baseado na estrutura do banco, chave-valor (key-value = Valkey). Inclusive a contribuição nesse projeto tem sido de empresas e pessoas que já apoiavam o projeto anteriormente, como AWS, Google e Oracle. Valkey foi lançado pela Linux Foundation.

[https://www.linuxfoundation.org/press/linux-foundation-launches-open-source-valkey-community](https://www.linuxfoundation.org/press/linux-foundation-launches-open-source-valkey-community)

[https://github.com/valkey-io/valkey](https://github.com/valkey-io/valkey)

## Garnet

Garnet é um projeto de pesquisa da Microsoft Research. É um armazenamento de cache remoto projetado para oferecer alto desempenho, extensibilidade e baixa latência. Garnet é escalonável por thread em um único nó. Ele também oferece suporte à execução de cluster fragmentado, com replicação, pontos de verificação, failover e transações. Ele pode operar na memória principal e também no armazenamento em camadas (como SSD e Azure Storage). Garnet suporta uma superfície de API rica e um modelo de extensibilidade poderoso. Garnet é compatível com Redis, pois utiliza o protocolo [RESP](https://redis.io/docs/reference/protocol-spec/).

[https://www.microsoft.com/en-us/research/project/garnet/](https://www.microsoft.com/en-us/research/project/garnet/)

[https://github.com/microsoft/garnet](https://github.com/microsoft/garnet)

## KeyDB

Outro projeto opensource que pode ser utilizado como alternativa ao Redis. Foi projetado para lidar com cargas de trabalho pesadas com benchmarking de nó único a mais de 1 milhão de operações/seg. KeyDB é um banco de dados multithread e superará o desempenho do Redis por nó segundo consta no próprio site. Existem diversas outras melhorias que o projeto também trouxe em relação ao Redis, como, replicação ativa e expiração de sub-chave.

[https://github.com/Snapchat/KeyDB](https://github.com/Snapchat/KeyDB)

[https://docs.keydb.dev](https://docs.keydb.dev/)

## DragonFly

Outra alternativa ao Redis que promete ter uma melhor performance e compatibilidade com as APIs do Redis. Segundo o próprio site:

>O Dragonfly foi projetado para oferecer a escalabilidade exigida pelas cargas de trabalho de dados modernas. Diga adeus aos ambientes complexos de vários nós com infinitas opções de configuração e infinitos pontos de falha. O Dragonfly pode ser facilmente ampliado para lidar com mais de 1 TB de dados e 4 milhões de QPS em um único nó, com um simples nó primário.

[https://www.dragonflydb.io](https://www.dragonflydb.io/)