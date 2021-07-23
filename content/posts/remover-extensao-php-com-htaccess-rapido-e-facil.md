---
title: Remover extensão .php com .htaccess (Rápido e fácil)
date: 2016-10-17T14:34:10-03:00
author: Sidnei Weber
layout: post
#cover: /uploads/2016/10/Seleção_012-220x162.png
tags:
  - php
---
Já havia tentado de várias maneiras remover a extensão .php dos meus arquivos, mas todas sem sucesso. Porém consegui fazer de uma maneira muito simples, editando o arquivo .htaccess. Lembrando que para funcionar a modulo "mod_rewrite" deve estar habilitado no seu servidor web.

Criaremos o arquivo .htaccess na raiz do nosso projeto com o seguinte conteúdo

```
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}.php -f
RewriteRule ^(.*)$ $1.php

</IfModule>
```

E quando formos criarmos nosso link no php, aqui no caso estamos com o exemplo signup.php, basta digitar assim:

```html
<a href="signup">signup here</a>
```

Sem a extensão .php. O servidor irá fazer todo o resto do trabalho.

[Fonte](http://www.codingcage.com/2015/11/how-to-remove-php-html-extensions-with.html)