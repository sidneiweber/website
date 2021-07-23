---
title: 'Utilizando o comando like com variável [PHP-MySQL]'
date: 2015-07-10T16:48:54-03:00
author: Sidnei Weber
layout: post
tags:
  - mysql
  - php
---

```php
include("conexao.php");
$busca = $_POST['busca'];
// comando like com variavel
// retorna todos os produtos que tenham o valor da variável busca em qualquer posição
$result = mysql_query("SELECT descricao FROM produtos WHERE descricao like '%".$busca."%' ");

// comando like normal
//retorna todos os nomes que tenham a palavra "pedro" em qualquer posição
$result = mysql_query(" SELECT nome FROM funcionarios WHERE nome like '%pedro%' ");</pre>
```