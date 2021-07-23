---
title: Instalar Java 8 no Manjaro/Archlinux
date: 2015-11-12T00:10:33-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2015/11/Java_620X0-220x162.jpg
tags:
  - arch
  - manjaro
  - java
  - linux
---
Primeiro atualize o sistema

```shell
pacman -Sy
```

Após isso baixamos a última versão (Na data da postagem essa era última versão)

```shell
wget http://javadl.sun.com/webapps/download/AutoDL?BundleId=111679 -O Java-latest
```

Descompactamos

```shell
tar -zxvf Java-latest
```

Copiamos para a pasta correta

```shell
cp -pr jre1.8.0_60 /opt
```

Criamos o link para o programa

```shell
ln -s /opt/jre1.8.0_60/bin/java /usr/bin/java
```

E o link para os plugins:

```shell
ln  -s /opt/jre1.8.0_65/lib/amd64/libnpjp2.so  ~/.mozilla/plugins/libnpjp2.so
```

[Fonte](http://www.unixmen.com/install-java-8-manjaroarchlinux/)