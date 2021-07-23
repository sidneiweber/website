---
title: Maiúsculas e Minúsculas em PHP
description: Maiúsculas e Minúsculas em PHP
date: 2015-07-06T15:47:02-03:00
author: Sidnei Weber
layout: post
tags:
  - php
---
$texto ="warquia pereira santos"

vamos transformar a variavel $texto

**=> Converte a string para letras minúsculas**

strtolower();  
```php
srttolower($texto);
```
vai sair assim = warquia pereira santos

**=> Converte a string para letras maiúsculas**  
strtoupper();
```php
srttoupper($texto);
```
vai sair assim = WARQUIA PEREIRA SANTOS

**=> Converte o primeiro caractere de uma string em maiúsculo**  
ucfirst();  
```php
ucfirst($texto);
```
vai sair assim = Warquia pereira santos

**=> Converte em maiúsculo o primeiro caractere de cada palavra contida em uma string**  
ucwords();  
```php
ex: ucwords($texto);  
```
vai sair assim = Warquia Pereira Santos

[Fonte](http://phpbrasil.com/artigo/bG73TMFSCr3H/maiusculas-e-minusculas-em-php)