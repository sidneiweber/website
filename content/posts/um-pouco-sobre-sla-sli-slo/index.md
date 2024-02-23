---
layout: post
title: Um pouco sobre SLA, SLI e SLO
description: 'Em um ambiente digital cada vez mais exigente e competitivo, a tríade de SLA, SLI e SLO emerge como um farol orientador para as organizações que buscam manter a confiança dos clientes e garantir a excelência operacional. Ao compreender e integrar esses elementos em suas práticas de gestão de serviços, as empresas podem não apenas atender, mas superar as expectativas dos clientes, estabelecendo-se como líderes em um mundo movido pela qualidade, confiabilidade e inovação.'
date: '2024-02-23 12:20:00'
tags: [devops, observabilidade, monitoramento, sre]
permalink: /um-pouco-sobre-sla-sli-slo/
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
author: Sidnei Weber
---
<!--more-->

## Introdução

Algumas métricas são importantes para entender como nossos serviços estão performando mas também existem métricas que nos dão uma visão sobre a confiabilidade e  disponibilidade desses serviços. Entre elas estão o SLI e SLO, que vamos entender um pouco mais nesse texto.

Primeiramente é necessário entender que são métricas e dados importantes para medir a confiabilidade dos serviços através da observabilidade, um dos principais pilares de SRE (Site Reliability Engineering). O conceito de engenharia de confiabilidade foi criado pela equipe de engenharia do Google.

## Exemplo prático

Vamos supor que o SLI seja a LATÊNCIA de uma API, o SLO seja de ter 97% das requisições abaixo de 1s nos últimos 30 dias, e o SLA firmado com o cliente é de responder em até 1s, 93% das requisições.

## SLI (Service Level Indicator)

São métricas que podem ser medidas (quantificáveis) em relação aos serviços, e que caso não estejam em bons números podem impactar a performance do serviço. Ex:

* Latência
* Taxa de erros
* Saturação
* Disponibilidade

Nem toda métrica pode se tornar um SLI, devemos entender as métricas que impactam a experiência do usuário ou o desempenho do sistema.

## SLO (Service Level Object)

Com o SLO podemos medir a maturidade dos serviços (internamente), é um valor na qual nos comprometemos em manter para não corromper a qualidade dos serviços e ambientes e também podendo dar direcionando nas ações. É importante que cada service level objective seja **atingível, repetitivo, mensurável, compreensível, significante e controlável**, além de conter as possíveis gratificações e penalidades previamente definidas para ambas as partes envolvidas.

É de suma importância que a organização esteja comprometida em manter os bons níveis do serviço e que o SLO não seja comprometido. Alguns exemplos:

* Disponibilidade:
  * SLI: Quantidade de erros em relação a quantidade de requests/processamentos.
  * SLO: 98% de requests/processamentos com sucesso.
* Latência:
  * SLI: Tempo de resposta ou tempo de processamento
  * SLO: 98% das requisições ou processamentos abaixo de X milissegundos.

### Como ter sucesso com SLI e SLO

* Considerar expectativas dos clientes (internos e externos);
* Deixar acordos de SLO bem claros;
  * O SLO deve ser visto como um recurso para garantir a melhoria contínua;
* Escolher bem as métricas para compor o SLI;

## SLA (Service Level Agreement)

Ainda temos o SLA que é um acordo entre quem está entregando o serviço e o usuário final, garantindo um certo nível de serviço (SLO).

O não cumprimento desse acordo (SLA), pode gerar consequências, dentre elas podemos citar a penalidade financeira, gerando descontos por exemplo para o cliente afetado.

Para mais informações sobre o assunto podemos conferir no material do Google https://sre.google/
