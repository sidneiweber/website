---
layout: post
title: Dica rápida - Verificar pastas que ocupam mais espaço
date: '2017-08-17 13:15:51 -0300'
author: Sidnei Weber
#cover: "/uploads/2016/12/Selecao_003.png"
tags: [linux, shell script]
---

Bom essa dica é bem simples mas muito útil, vamos usar uma combinação de comandos para listar as 5 pastas que mais utilizam espaço no diretório corrente. O bom comando seria esse:

```shell
du -Sh | sort -rh | head -5
```

#### Saida:

```shell
2,1G	./log/journal/457f9ec73a36473ab3a70f2a1ebce863
196M	./lib/mysql
189M	./lib/docker/aufs/diff/105ad3b8329fb9264b17543f7d70746b1c330f18523f27cdee5ad3fdff966697/usr/bin
116M	./lib/nvidia
111M	./lib/docker/aufs/diff/105ad3b8329fb9264b17543f7d70746b1c330f18523f27cdee5ad3fdff966697/usr/share/cattle/0e44936b6b56ae4372799b0f48e6e934/WEB-INF/lib
```

#### Explicando:

O comando **du** faz a verificação do uso das pastas em si;  
O comando **sort** faz a ordenação do maior pro menor;  
E o comando **head** mostra os 5 primeiros, pode ser usado qualquer quantidade.

Se alguém tiver dúvidas quanto as opções usadas, basta digitar o nome do comando &#8211;help, que terá todas as informações.
