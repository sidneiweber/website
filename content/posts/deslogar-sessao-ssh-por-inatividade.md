---
title: Deslogar sessão ssh por inatividade
date: 2016-09-19T13:56:05-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/09/Artigo_07_002-220x162.jpg
tags:
  - ssh
---
Para deixar a configuração do servidor ssh um pouco mais segura, podemos configurar uma opção para um usuario que ficar inativo por um determinado tempo seja deslogado automaticamente. É uma opção muito útil caso seja esquecido uma sessão aberta, o que é bastante perigoso por sinal.

O arquivo que devemos alterar é o **/etc/ssh/sshd_config**, adicionando ou alterando os seguintes parametros:

```shell
ClientAliveCountMax 0
ClientAliveInterval 30
```

Com isso, caso tenha inatividade na sessão por 30 segundos o usuário será deslogado.

**ClientAliveCountMax**: Define o numero máximo de envio de pacotes para saber se o cliente está ou não ativo.  
**ClientAliveInterval:** Define um intervalo de tempo em segundos após o qual, se o terminal estiver ocioso, será finalizada a sessão.