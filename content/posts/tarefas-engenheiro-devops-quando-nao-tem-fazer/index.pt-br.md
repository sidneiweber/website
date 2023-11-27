---
layout: post
title: Tarefas para um engenheiro DevOps fazer quando não tiver o que fazer
description: 'Por mais que você possa sempre estar atarefado, existem momentos em que você pode não estar tocando um projeto especifico ou grande. Para evitar aquele sentimento de não estar contribuindo, seguem minhas pequenas dicas para se manter na ativa e mostrando serviço.'
date: '2023-11-27 09:00:00'
tags: [devops]
author: Sidnei Weber
#url: /tarefas-para-engenheiro-devop/
resources:
- name: "featured-image"
  src: "featured-image.jpg"
---
<!--more-->


# Documentar

Possivelmente nem todo mundo gosta de escrever documentação, mas é algo muito necessário. Aproveitar o tempo "livre" para documentar o que está fazendo ou já fez, pode trazer diversos benefícios. Durante o processo de documentação, você também estará revisando e ajudando a fixar o que fez. Pode encontrar algum ponto de melhoria no projeto, por que não, nem tudo é perfeito.

Sua equipe também pode se beneficiar disso, tendo materiais de consulta sempre que necessário, diminuindo as interrupções por perguntas básicas sobre algum projeto. Por isso é muito importante documentar e também manter as documentações atualizadas, pior do que não documentar é ter algo documentado errado.

# Conversar com pessoas

Esse é um ponto que às vezes esquecemos de visitar, estamos tão atarefados ou imersos em um problema técnico que esquecemos das pessoas. Converse com pessoas do seu e de outros times para entender os problemas que eles estão enfrentando, talvez você saiba como ajudá-los. Converse sobre as coisas que você já fez, saiba se está sendo útil, se não há nada que possa ser melhorado.

Tenho certeza que sempre tem alguém enfrentando algum problema ou executando uma tarefa que poderia facilmente ser automatizada ou otimizada.

# Analisar suas métricas

Deixa eu adivinhar, você já montou sua estrutura de observabilidade, como por exemplo o Prometheus e Grafana? Se sua resposta for sim, provavelmente há várias métricas para serem analisadas. Analisar como anda os recursos dos servidores e serviços pode trazer vários pontos de melhorias ou algum serviço que esteja se degradando com o passar do tempo. Analise, descubra o motivo e encontre uma solução

Bora analisar essas métricas e trazer melhorias?

# Otimizar os custos

![Não gaste seu dinheiro à toa](pica-pau.png "Money")

Eis um ponto indiscutível, ninguém gosta de gastar mais do que deveria. Não seria diferente no mundo da tecnologia (mesmo para aquela empresa enorme e cheio de investimentos), principalmente quando estamos trabalhando com serviços de cloud.

Verifica onde está sendo o maior gasto, analise se não há maneiras de diminuir o custo (obviamente sem perder performance). Alguns pontos que podem ser relevantes:

* Dimensionamento incorreto de CPU e memória;
* Muito tráfego de rede entre zonas de disponibilidade;
* Excesso de consultas a banco de dados;
* Ambientes de desenvolvimento ligados fora do horário de trabalho;

Esse são somente alguns pontos, ainda podem existir diversos outros. Não esquecendo que é possível montar relatórios de custos por time ou serviço, trazendo uma visão muito mais simplificada dos gastos.

# Prova de conceito

Sabe aquela ferramenta ou tecnologia que você acha que pode trazer melhorias? É um bom momento para realizar sua POC e validar se ela pode trazer algum benefício para os ambientes. Sempre importante validar durante os testes se ela vai trazer alguma redução de custo, otimização de processos ou mesmo simplificar alguma tarefa.

Mas lembre-se, é muito importante que essa POC tenha como objetivo trazer algum resultado, converse com seus colegas e gestores para validar suas ideias. Nada de querer brincar com aquela ferramenta só por que todo mundo diz que é boa ou empresa X está usando.

# Passagem de conhecimento

Além da parte de documentação citada anteriormente, também é possível realizar apresentações/workshops com seu time ou empresa para passar conhecimento, mostrar os serviços realizadas e as melhorias que você vem trazendo. Dê muita atenção aos colegas de time que estão começando, dedique um tempo para ensiná-los da forma correta, tenho certeza que muitos lembrarão de você quando estiverem em outras empresas.

Também é possível passar conhecimento para pessoas de fora da sua empresa, como em artigos (como este), vídeos no Youtube ou participação em fóruns e conferências. Lembre-se, quem não é visto não é lembrado ;). 

# Testes de caos

![Não fique perdido](chaos.jpg "Chaos")

O que aconteceria se o principal banco de dados da sua empresa ficasse fora? Não sabe, então esse é um bom motivo para criar teste de caos. Em um ambiente muito parecido com produção, simule problemas e analise o comportamento. O sistema fica indisponível? Existem alternativas para que isso não ocorra? Existe formas de minimizar os impactos? São várias perguntas que podem ser resolvidas durante os testes.

# Contribuir para projetos de código aberto

Você já fez tudo que foi listado aqui e ainda sente pode contribuir mais? Bom, talvez exista um projeto de código aberto que você possa contribuir. Existem diversos projetos muito bons precisando de ajuda. Ou talvez você queira iniciar seu próprio projeto de código aberto, por que não?

Em qualquer um dos casos o ganho de experiência e conhecimento será inestimável, além de ajudar a criar novas conexões e de certa forma divulgar um pouco do seu trabalho.