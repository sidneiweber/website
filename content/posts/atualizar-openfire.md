---
id: 40
title: Atualizar Openfire
date: 2009-09-29T12:19:12-03:00
author: Sidnei Weber
layout: post
tags:
  - openfire
---
Uma dúvida muito constante dos usuários Openfire é como fazer para atualizarem seu servidor para uma versão mais recente.

Segue então um pequeno howto (adaptado do orginal da Ignite Realtime):

* Pare o Openfire  
* Faça um backup do diretório de instalação do Openfire (isso é preciso porque ao abrir o novo .tar.gz ou .zip os dados serão sobreescritos). No meu caso, que mantenho o openfire no /opt, um simples mv /opt/openfire /opt/openfire.old já resolve.  
* Backupeie o banco de dados (se você usar o DB interno, isso já foi feito no passo anterior). Se você usa MySQL, por exemplo, um simples mysqldump da base já resolve.  
* Abra o .tar.gz ou .zip (isso irá criar um novo diretório openfire, se você moveu o anterior, como eu costumo fazer)  
* Copie o diretório conf do backup para a nova instalação.  
* Se você usar o DB interno, copie o diretório embedded-db do backup para a nova instalação.  
* Copie o diretório enterprise do backup para a nova instalação (se ele existir)  
* Copie o diretório plugins do backup para a nova instalação, exceto por \_plugins/admin\_ (esse passo eu dispenso, e sempre instalo os plugins novamente, já que as configurações e dados dos mesmos estão no DB)  
* Copie os arquivos modificados localizados em resources/security do backup para a nova instalação.  
* Inicie o Openfire.  
*

Voilà. Seu servidor está atualizado e no ar novamente.

FONTE: http://mundoopensource.blogspot.com/2008/08/openfire-como-atualizar-o-servidor-para.html