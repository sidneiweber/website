---
layout: post
title: Rarcrack - Quebrando senhas de arquivos rar, 7z e zip
date: '2013-04-23 11:58:04 -0300'
author: Sidnei Weber
tags:
- linux
---

Ontem fiz download de um arquivo RAR e para minha surpresa o mesmo estava com senha. Antes de procurar outros links resolvi tentar quebrar a senha deste arquivo mesmo, e buscando uma solução no Google eis que acho o _rarcrack_, um brute force para arquivos .rar, .zip e 7-zip.

Faça download do arquivo em:

  * <http://sourceforge.net/projects/rarcrack/files/rarcrack-0.2/%5BUnnamed%20release%5D/>

Feito o download, como root, navegue até a pasta onde salvou o arquivo e faça:

```shell
tar -xjf rarcrack-VERSAO.tar.bz2  
cd rarcrack-VERSAO
```

Você precisará do gcc ou outro compilador C:

```shell
make  
make install
```

Pronto, instalado.

Para usar o rarcrack:

```shell
rarcrack --type *rar nomedoarquivo.rar
```

* o type fica a seu critério, dependendo do seu arquivo compactado.

Obs.: Infelizmente acabei não lembrando de cronometrar o processo. Mas após 30~45min o arquivo já estava extraído.

[Fonte](http://www.vivaolinux.com.br/dica/Rarcrack-Quebrando-senhas-de-arquivos-rar-7z-e-zip)
