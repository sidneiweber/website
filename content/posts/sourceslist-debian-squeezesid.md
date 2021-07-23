---
title: sources.list | Debian Squeeze/Sid
description: sources.list | Debian Squeeze/Sid
date: 2009-07-20T13:02:45-03:00
author: Sidnei Weber
layout: post
tags:
  - debian
---
Descomente os que você quiser

```bash
# deb cdrom:[Debian GNU/Linux testing \_squeeze\_ &#8211; Official Snapshot i386 CD Binary-1 20081201-10:57]/ squeeze main  
#deb cdrom:[Debian GNU/Linux testing \_squeeze\_ &#8211; Official Snapshot i386 CD Binary-1 20081201-10:57]/ squeeze main  
#deb-src http://ftp.br.debian.org/debian/ squeeze main  
#deb-src http://security.debian.org/ squeeze/updates main  
#deb http://ftp.br.debian.org/debian/ squeeze main

# mains contribs non-frees  
deb http://ftp.br.debian.org/debian/ squeeze main contrib non-free  
deb http://security.debian.org/ squeeze/updates main contrib non-free
#media  
deb http://www.debian-multimedia.org squeeze main  
# Experimental Staging  
deb http://www.debian-multimedia.org experimental main

#virtualbox  
deb http://download.virtualbox.org/virtualbox/debian lenny non-free

#debian experimental kernels  
deb http://kernel-archive.buildserver.net/debian-kernel/ trunk main

# Debian Volatile http://www.debian.org/volatile/  
#deb http://volatile.debian.org/debian-volatile squeeze/volatile main  
#deb-src http://volatile.debian.org/debian-volatile squeeze/volatile main  
deb http://volatile.debian.org/debian-volatile lenny/volatile main contrib non-free

# Google testing repository  
#deb http://dl.google.com/linux/deb/ testing non-free  
#(wget -q -O – https://dl-ssl.google.com/linux/linux\_signing\_key.pub | sudo apt-key add – )

# swiftfox http://www.getswiftfox.com/index.htm  
#deb http://getswiftfox.com/builds/debian unstable non-free

#compiz-fusion  
deb http://apt-get.if.uff.br lenny-ifuff compiz

### KDE 4.2  
#SID(Precisa porquê várias dependências estão aqui)  
#deb http://ftp.de.debian.org/debian/ sid main contrib non-free  
#deb-src http://ftp.de.debian.org/debian/ sid main contrib non-free

#Experimental(Repositório onde se encontra o KDE4)  
#deb http://ftp.de.debian.org/debian/ experimental main  
#deb-src http://ftp.de.debian.org/debian/ experimental main
```
