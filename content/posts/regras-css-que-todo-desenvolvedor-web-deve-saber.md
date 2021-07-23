---
title: 11 regras CSS que todo desenvolvedor web deve saber
date: 2015-07-06T15:43:13-03:00
author: Sidnei Weber
layout: post
tags:
  - css
---

### @media

A regra @media não só nos permite definir estilos para nossa página da web quando impressa na tela. Hoje em dia esta regra atingiu um patamar mais avançado com as “media queries“, que são associadas à criação de sites “adaptativos” ou “responsivos”, ou seja, que se ajustam ao tamanho da tela do dispositivo no qual estiverem sendo acessados.

Você pode criar uma media query usando propriedades como min-width para ajustar seu layout de acordo com o tamanho do “viewport” do dispositivo do usuário.

```css
@media screen and (max-width: 960px) {
// seus estilos vão aqui ....
}
```

### background-size

Aí está uma propriedade CSS3 muito útil que ganhou total suporte nos navegadores modernos. Antes, para fazer com que uma imagem, definida como background, fosse ajustada 100% (full size), de acordo com o tamanho do elemento que faz parte, era necessário algumas linhas de código, hacks e até javascripts. Agora nós precisamos de apenas uma linha de código, ou melhor, apenas uma propriedade:`background-size`

```css
body {
background: url(image.jpg) no-repeat;
background-size: 100%;
}
```

### @font-face

```css
@font-face {
font-family: Blackout;
src: url("assests/blackout.ttf") format("truetype");
}
```

### overflow:hidden

```css
.container {
overflow: hidden;
}
```

Existe uma série de soluções e hacks para ajudar a resolver o problema com os elementos flutuantes. Este problema ocorre quando temos, dentro de um elemento de bloco (um DIV por exemplo), um outro elemento com float. Esse elemento que “flutua” não força automaticamente a altura do elemento container, ou seja, a altura do elemento container não acompanha a altura do elemento com float. Isto ocorre porque o elemento que está flutuando deixa de considerar o elemento container como pai.

Um jeito hiper simples de “limpar” os elementos flutuantes é simplesmente declarar `overflow: hidden;` no elemento container. Isto é muito interessante, porque não precisamos mais adicionar “lixo”  no nosso código html, como o tão famoso `<br style="clear:both" />`.

### margin: 0 auto;

```css
#container {
    margin: 0 auto;
}
```

É surpreendente que até então não exista nenhuma propriedade especifica para centralizar elementos. Mas enquanto isso não acontece, nós podemos utilizar o recurso de margem automática. Ao adicionarmos `margin: 0 auto;` em um elemento de bloco, este será centralizado.

### input[type="text"]

O seletor `input[type="text"]`, bem como outros seletores avançados no geral, irão elevar suas habilidades com CSS do nível intermediário para o avançado. Seletores de atributos, em particular, são extremamente úteis para estilizar elementos sem que precisemos criar classes adicionais.

Você poderá manipular não só campos do tipo texto, mas também radio, checkbox, submit, password, file, etc.

```css
input[type="text"] {
width: 200px;
}

input[type="checkbox"]{
margin-right: 1em;
}
```

[Fonte](http://wpmidia.com.br/desenvolvimento-web/11-regras-css-todo-desenvolvedor-web-deve-saber/)