---
layout: post
title: Como redimensionar volume EBS no Linux sem downtime
description: Quanto acaba o espaço de uma instância Linux em produção, não podemos desligar ou reiniciar para dar manutenção, vamos ver como redimensionar volume principal no Linux EC2 sem precisar reiniciar, o famoso root. Sem nenhum downtime, com alguns poucos comandos.
date: '2020-03-13 21:54:28'
tags: [aws, ec2, linux]
#cover: "/img/ebs/ebs.png"
author: Sidnei Weber
---

Esse processo pode ser feito sempre que precisar aumentar o volume sem precisar desligar a instância ou desanexar o volume.

Alterações em produção? Nesse caso sim :)

- Após estar logado em sua conta AWS vamos escolher a opção EC2 na lista de serviços
- Clicamos em "Volumes" no menu **"ELASTIC BLOCK STORE"**
- Escolha o volume que deseja redimensionar e com o botão direito do mouse clique em **"Modify Volume"**

Verá uma janela como essa:
![Modify Volume ><](/img/ebs/modify-volume.png)

- Defina o novo tamanho para o volume, como no exemplo da imagem estamos estendendo o volume para 20GB
- Confirme no botão "Modify"

Agora precisaremos extender a partição no sistema. Acesse a instância via SSH e rode o comando abaixo para listar o dispositivos.

```shell
ubuntu@ip-172-31-23-184:~$ lsblk 
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0     7:0    0 89.1M  1 loop /snap/core/8268
loop1     7:1    0   18M  1 loop /snap/amazon-ssm-agent/1480
xvda    202:0    0   20G  0 disk 
└─xvda1 202:1    0    8G  0 part /
```

Podemos ver o disco está com o novo tamanho (20GB), porém a partição continua com o tamanho antigo (8GB) e precisa ser estendido.

Para isso vamos usar o comando (growpart):

```shell
ubuntu@ip-172-31-23-184:~$ sudo growpart /dev/xvda 1
CHANGED: partition=1 start=2048 old: size=16775135 end=16777183 new: size=41940959,end=41943007
```

Levando em considreção o dispositivo e o número da partição. Vamos chegar novamente o tamanho da partição:

```shell
ubuntu@ip-172-31-23-184:~$ lsblk 
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0     7:0    0 89.1M  1 loop /snap/core/8268
loop1     7:1    0   18M  1 loop /snap/amazon-ssm-agent/1480
xvda    202:0    0   20G  0 disk 
└─xvda1 202:1    0   20G  0 part /
```

Podemos notar que a partição também está estendida, mas se olharmos o uso do disco podemos notar que o mesmo não foi alterado, isso porque ainda precisamos estender o sistema de arquivos.

```bash
ubuntu@ip-172-31-23-184:~$ df -Th |grep /dev/xvda1
/dev/xvda1     ext4      7.7G  1.1G  6.7G  14% /
ubuntu@ip-172-31-23-184:~$ 
```

E para estender o sistema de arquivos é simples, se o sistema de arquivos que está usando é ext2, ext3 ou ext4, basta executar o comando:

```shell
ubuntu@ip-172-31-23-184:~$ sudo resize2fs /dev/xvda1 
resize2fs 1.44.1 (24-Mar-2018)
Filesystem at /dev/xvda1 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 3
The filesystem on /dev/xvda1 is now 5242619 (4k) blocks long.
```

Agora se olharmos novamente o espaço utlizado com o comando df:

```shell
ubuntu@ip-172-31-23-184:~$ df -Th |grep /dev/xvda1
/dev/xvda1     ext4       20G  1.1G   19G   6% /
ubuntu@ip-172-31-23-184:~$ 
```

Pronto, volume estendido com 0 downtime. Bom proveito!
