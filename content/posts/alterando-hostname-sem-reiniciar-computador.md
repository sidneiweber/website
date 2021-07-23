---
id: 373
title: Alterando hostname sem reiniciar computador
date: 2017-02-03T14:15:55-03:00
author: Sidnei Weber
layout: post
#cover: /img/uploads/2016/05/cropped-captura_de_tela-300x167.jpg
tags: [linux]
---
Para podermos alterar o nome da máquina sem precisar reiniciar é muito simples. Primeiramente precisamos alterar o arquivo /etc/hostname. Mas após a alteração notamos que o nome não muda, mesmo dando o comando hostname, o nome continua o antigo. Para essa alteração valer sem precisar reiniciar, pois as vezes pode se tratar de um servidor que não pode parar no momento, basta digitar o comando abaixo:

```shell
echo "novo-hostname" > /proc/sys/kernel/hostname
```

Pronto, problema resolvido.

[Fonte](https://www.vivaolinux.com.br/dica/Debian-Wheezy-Alterando-hostname-sem-reiniciar)