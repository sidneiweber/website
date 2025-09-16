---
title: Atualizar WordPress localhost
date: 2015-07-06T15:41:19-03:00
author: Sidnei Weber
layout: post
url: atualizar-wordpress-localhost
tags:
  - wordpress
---
Para atualizar o WordPress instalado localmente, basta trocar o método do sistema de arquivos com o parâmetro:  **FS_METHOD**

A seguir, estão as constantes válidas para atualizações do WordPress:

  * **FS_METHOD** obriga o método do sistema de arquivos. Deve ser "direct", "ssh", "ftpext", ou "ftpsockets". Geralmente, você deve apenas mudar isso se estiver enfrentando problemas de atualização, se mudar, e não adiantar **mude de volta/remova**, Na maioria das circunstâncias, definir **ftpsockets** vai funcionar se o método escolhido automaticamente não funcionar. 
      * **(Primary Preference) "Direct"** obriga a usar solicitações Direct File I/O de dentro do PHP, o que possui muitas questões de segurança em servidores mal configurados, pode ser escolhido automaticamente quando necessário.
      * **(Secondary Preference) "ssh"** obriga a usar a extensão SSH PHP.
      * **(3rd Preference) "ftpext"** obriga a usar a extensão FTP PHP para acesso FTP e finalmente:
      * **(4th Preference) "ftpsockets"** usa classe de Sockets PHP para acesso FTP.

```php
define('FS_METHOD', direct);
```