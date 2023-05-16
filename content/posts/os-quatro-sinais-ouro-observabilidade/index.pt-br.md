---
layout: post
title: Os quatro sinais de ouro da observabilidade
description: 'Muito além das métricas de CPU e memória podemos ter métricas efetivas e que nos ajudam a entender a saúde do nosso ambiente e resolver problemas de forma mais rápida caso eles ocorram..'
date: '2023-02-02 16:00:00'
tags: [observabilidade, devops, sre]
author: Sidnei Weber
resources:
- name: "featured-image"
  src: "featured-image.jpg"
---

Muito além das métricas de CPU e memória podemos ter métricas efetivas e que nos ajudam a entender a saúde do nosso ambiente e resolver problemas de forma mais rápida caso eles ocorram.

Nos primórdios (que não faz tanto tempo assim) as aplicações tinham pouca ou quase nenhuma monitoração. Quando tinham alguma monitoração, tinhamos somente informações básicas de hardware, rede e com alguma sorte quando algum serviço ficava indisponivel. Com a mudanças de serviços únicos (monolitos) para centenas ou até milhares de microserviços, ambientes complexos, velozes, na nuvem e com uma visibilidade cada vez mais difícil, foi necessário mudar os padrões de indicadores para algo mais efetivo para quem mais sofre, o usuário/cliente.

Após o Google disponibilizar seu [livro sobre SRE](https://sre.google/sre-book/table-of-contents/) a engenharia de confiabilidade vem se tornando algo cada vez mais presente e necessário. Um dos principais pontos do livro e que iremos comentar aqui é sobre os [quatro sinais de ouro](https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals) da observabilidade.

## O que é

Como o próprio nome sugere, os quatro sinais de ouro são os pontos cruciais a serem observados em um ambiente ou sistema que podem afetar diretamente o cliente ou pontos muito importantes que podem impactar no todo. A coleta dessas métricas é importante para nos ajudar a resolver problemas, criar alertas e ajudar no planejamento de capacidade dos recursos.

### Latência

A latência é uma métrica que impacta diretamente o usuário, ela mede o tempo que um sistema demora para responder o cliente. Vale deixar claro que é importante medir a latência das requisições com sucesso e com erro, pois um erro pode retornar de maneira rápida e acabar afetando os valores medidos.

### Tráfego

Nessa métrica medimos basicamente a quantidade de acessos que um sistema ou site recebe, mas pode depender do tipo do sistema que estamos falando, como por exemplo um serviço de streaming, onde o tráfego pode ser baseado na taxa de rede ou sessões simultâneas.

### Erros

Aqui medimos a quantidade de erros de um sistema ou aplicação. Vale ressaltar que não são somente os erros HTTP ([4XX e/ou 5XX](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status)) que se enquadram aqui, podem ser erros de negócio, requisições que ultrapassem deterimado tempo, etc.

### Saturação

Na saturação podemos saber o quanto os recursos estão ocupados, como CPU, memória e disco por exemplo, e novamente, tudo depende do ambiente envolvido. Se o sistema exigir mais memória, devemos medir com mais atenção esse ponto. Outro ponto muito importante é testar e saber em que ponto o sistema começa a sofrer com degradação, se é perto dos 100% ou é antes dos 90% ou após determinado período. A saturação também pode ajudar com previsões de problemas, como o disco ficará cheio dentro de 2 dias, baseado nas métricas que já possui.

É claro que essas informações sozinhas podem não entregar valor mas bem aplicadas e organizadas podem gerar muita confiança nos sistemas observados. Temos ainda diversos outros pontos que podem complementar esses pontos vistos aqui, como logs, traces, a própria ferramenta de alertas, entre outros. Mas veremos esses pontos em outra oportunidade.

Fonte: https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals