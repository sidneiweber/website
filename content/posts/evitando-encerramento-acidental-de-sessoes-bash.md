---
title: 'Evitando encerramento acidental de sessões bash'
date: 2015-07-06T15:50:35-03:00
author: Sidnei Weber
layout: post
tags:
  - linuxss
  - servidores
  - shell script
---
A sequência de teclas "`CTRL+D`" encerra uma sessão bash. Às vezes digitamos estas teclas por acidente e encerramos uma sessão acidentalmente.

Para evitar que isto ocorra, definimos a variável de ambiente `IGNOREEOF`:

```shell
export IGNOREEOF=1
```

Desta forma, para encerrar uma sessão bash, precisamos digitar a sequência `CTRL+D` duas vezes ou então digitar `exit`.

Esta variável de ambiente deve ser definida no arquivo `.bashrc`.

Fonte: [Dicas-L](http://www.dicas-l.com.br/arquivo/encerramento_acidental_de_sessoes_bash.php#.VDVCZdRdW2w)