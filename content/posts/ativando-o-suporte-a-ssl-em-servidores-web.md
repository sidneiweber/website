---
title: Ativando o suporte a SSL em servidores web
date: 2013-04-23T11:55:16-03:00
author: Sidnei Weber
layout: post
tags:
  - servidores
---
O SSL (Secure Socket Layer) é o protocolo usado para criar páginas seguras, encriptando toda a transmissão entre o cliente e o servidor. Os dois usos mais comuns são em páginas de comércio eletrônico, onde é necessário oferecer um ambiente seguro para concluir a transação e transmitir dados confidenciais e também na criação de ambientes administrativos, como os usados pela maioria dos gestores de conteúdo, que permitem que você gerencie o conteúdo do site.

Na grande maioria das distribuições, o pacote com o mod_ssl é instalado juntamente com o pacote principal do Apache, ou é pelo menos disponibilizado como um pacote separado, instalável através do gerenciador de pacotes.

No caso das distribuições derivadas do Debian, você precisa apenas ativar o módulo usando o comando "a2enmod". Reinicie o serviço para que a alteração entre em vigor:

```shell
a2enmod ssl
/etc/init.d/apache2 force-reload
```

No caso do CentOS, é necessário instalar o pacote "mod_ssl" usando o yum. O script de pós-instalação se encarrega de adicionar o script de carregamento na pasta "/etc/httpd/conf.d" automaticamente, concluindo a instalação. Não se esqueça de reiniciar o serviço para que a alteração entre em vigor:

```shell
yum install mod_ssl
service httpd restart
```

Com o módulo carregado, fica faltando apenas o componente mais importante, que é o certificado SSL propriamente dito.

Se você quer ativar o SSL para testes ou para uso interno (para acessar alguma ferramenta administrativa instalada no servidor, ou para uso em uma página disponibilizada apenas para um grupo de amigos, por exemplo), você pode simplesmente gerar seu próprio certificado, o que é rápido, grátis e indolor. Se, por outro lado, você está ativando o SSL para uso em um site de comércio eletrônico, é necessário obter um certificado reconhecido através da Verisign ou outra entidade certificadora.

Os certificados caseiros são chamados de certificados self-signed (auto-assinados), já que você mesmo faz o papel de entidade certificadora, gerando e assinando o certificado. O algoritmo de encriptação usado é o mesmo, de forma que um certificado self-signed corretamente gerado oferece a mesma segurança que um certificado reconhecido. O grande problema é que os navegadores nos clientes não serão capazes de verificar a autenticidade do certificado, de forma que os visitantes receberão um aviso de "certificado não reconhecido" ao acessarem a página:

![ssl ><](https://www.metaweb.com.br/imagens/produtos/ssl.png)

O propósito de entidades certificadoras, como a Verisign, é confirmar a titularidade dos certificados, confirmando que o certificado recebido ao acessar determinado site pertence realmente à entidade que o está fornecendo. É isso que garante que você está mesmo acessando o home banking do banco em que tem conta e não o site de um script kiddie qualquer. Certificados assinados por entidades certificadoras são automaticamente aceitos pelos navegadores (já que sua identidade já foi confirmada pela entidade certificadora), o que evita a exibição da mensagem.

Vamos então começar com a configuração de um certificado self-signed, e em seguida entender o que muda ao utilizar um certificado reconhecido.

[Fonte:](http://www.hardware.com.br/dicas/ssl-servidores-web.html)