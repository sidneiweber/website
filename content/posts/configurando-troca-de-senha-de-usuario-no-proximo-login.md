---
title: Configurando troca de senha de usuário no próximo login
date: 2014-01-12T18:51:44-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
---
Para isso, utilizaremos o comando chage. Antes, listamos as propriedades de login deste usuário: 

```shell
chage -l
```

Exemplo: 

```shell
chage -l perm

  Última mudança de senha                              : Jan 07, 2014
  Senha expira                                         : nunca
  Senha inativa                                        : nunca
  Conta expira                                         : nunca
  Número mínimo de dias entre troca de senhas          : 0
  Número máximo de dias entre troca de senhas          : 99999
  Número de dias de avisos antes da expiração da senha : 7
```

Como podemos visualizar, a senha deste usuário &#8220;nunca expira&#8221;. Então, forçaremos a expiração de senha para o próximo login que este usuário venha fazer, executando o comando a seguir: 

```
chage -d 0 perm
```

Fonte: <http://www.vivaolinux.com.br/dica/Configurando-troca-de-senha-de-usuario-no-proximo-login>