---
title: Alerta de espaço em disco via e-mail
description: Alerta de espaço em disco via e-mail
date: 2015-07-06T15:39:40-03:00
author: Sidnei Weber
layout: post
tags:
  - shell script
---
Script para enviar e-mail quando o uso de disco chegar a 90% de uso

```shell
df -k | grep -e 'lv' | awk '{ print $4 " " $7 }' | while read output;
do
echo $output
usep=$(echo $output | awk '{ print $1}' | cut -d'%' -f1 )
partition=$(echo $output | awk '{ print $2 }' )
if [ $usep -ge 90 ]; then
echo "Verifique o diretorio "$partition" com ($usep%) de uso no servidor $(hostname)" | mail -s "Alerta! Disco excedido em $usep%" seu_email@provedor.com
fi
done
```