---
title: Validando Formulários PHP
description: Validando Formulários PHP
date: 2013-04-23T11:49:34-03:00
author: Sidnei Weber
layout: post
tags:
  - php
---

## <span id="Validando_CEP">Validando CEP</span>

O código abaixo valida o CEP informado através do método POST (conforme script abaixo).

```php    
<?php
<span style="font-family:Consolas, Monaco, monospace;">
// Validando CEP</span>
if($_POST['cep']){

if (!eregi("^[0-9]{5}-[0-9]{3}$", $_POST['cep'])) {
echo "<script language="JavaScript">alert('CEP inválido !!!');</script>";

}else{
echo "O CEP $cep foi validado com sucesso!!!";
}
}
?>
```

Onde informamos que o CEP foi validado com sucesso, tu poderá customizar de acordo com suas necessidades!

## <span id="Validando_Data">Validando Data</span>

O código abaixo valida a Data informada através do método POST (conforme script abaixo).

```php
<?php

// Validando Data
if($_POST['data']){
if (!eregi("^[0-9]{2}/[0-9]{2}/[0-9]{4}$", $_POST['data'])) {
echo "<script language="JavaScript">alert('Data inválida !!!');</script>";

}else{
echo "A Data $data foi validada com sucesso!!!";
}
}
?>
``` 
    

Onde informamos que validação da Data foi efetuada com sucesso, tu poderá customizar de acordo com suas necessidades!

## <span id="Validando_Endereco_de_EMail">Validando Endereço de EMail</span>

O código abaixo deverá validar o Endereço de Email informado através do método POST (conforme script abaixo).

```php 
<?php

// Validando o EMail
if ($_POST['email']){

if (!eregi("^[a-z0-9_.-]+@[a-z0-9_.-]*[a-z0-9_-]+.[a-z]{2,4}$", $_POST['email'])) {
echo "<script language="JavaScript">alert('Email inválido !!!');</script>";

}else{
echo "O EMail $email foi validado com sucesso!!!";

}
}
?>
```

Onde informamos que validação do EMail foi efetuado com sucesso, tu poderá customizar de acordo com suas necessidades!

## <span id="Validando_Telefones">Validando Telefones</span>

Abaixo temos 2 exemplos de validação de telefone.

Exemplo 01:

```php
<?php

// Validando Telefone no Formato xxxx-xxxx
if($_POST['telefone']){

if (!eregi("^[0-9]{4}-[0-9]{4}$", $_POST['telefone'])) {
echo "<script language="JavaScript">alert('Telefone inválido !!!');</script>";

}else{
echo "O Telefone $telefone foi validado com sucesso!!!";

}
}
?>
```    

Exemplo 02:

```php 
<?php

// Validando Telefone no Formato (DDD) xxxx-xxxx
if($_POST['telefone']){

if (!eregi("^([0-9]{3}) [0-9]{4}-[0-9]{4}$", $_POST['telefone'])) {
echo "<script language="JavaScript">alert('Telefone inválido !!!');</script>";

}else{
echo "O Telefone $telefone foi validado com sucesso!!!";
<span style="font-family:Consolas, Monaco, monospace;">}</span>
}
?>
```

Onde informamos que a validação do Telefone foi efetuada com sucesso, tu poderá customizar de acordo com suas necessidades!

## <span id="Validando_Pagina">Validando Página</span>

Esse script é especial e necessário para quem é desenvolvedor web!

Imagine aquele sistema enorme que você produziu e deixou no Servidor de seu cliente?

Após receber o conteúdo ele vende ou fornece cópia para um “amigo” que quer uma solução similar!

Criptografando o código e colocando este conteúdo, você garantirá a segurança de suas informações!

Vamos dar uma olhada no código?

```php
<?php

<span style="font-family:Consolas, Monaco, monospace;">// Valida a cópia do Sistema</span>

$server = $_SERVER['SERVER_NAME'];
$endereco = $_SERVER ['REQUEST_URI'];
$url ="http://" . $server . $endereco;

$url_certa = "http://enderecodosite.com/";

if($url != $url_certa) {
echo "<script language="JavaScript">alert('VOCÊ NÃO TEM LICENÇA PARA USAR ESTA LOJA - Entre em contato com o EMail email@seuemail.com.br para validar sucópia.');</script>";

}else{

echo "COLOQUE AQUI O CÓDIGO DA PÁGINA";
}
<span style="font-family:Consolas, Monaco, monospace;">?>

<a href="http://www.webmaster.pt/como-validar-formulario-8269.html">Fonte</a></span>
```