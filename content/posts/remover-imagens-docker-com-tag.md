---
title: 'Remover imagens Docker com TAG "none"'
date: 2020-07-06T15:50:35-03:00
description: Imagens que não são usadas e acabam nos atrapalhando.
author: Sidnei Weber
layout: post
#cover: /img/docker.png
tags:
- docker
---

Primeiro vamos fazer uma pesquisa sobre nossas imagens usando: **docker images -a**

```shell
$ docker images -a
sonarqube                                                   0.8                 34e889c54fe4        3 weeks ago         536MB
<none>                                                      <none>               2e0435619f04        3 weeks ago         536MB
<none>                                                      <none>               e08451c98fcb        3 weeks ago         536MB
<none>                                                      <none>               3cbb4fa1d367        3 weeks ago         510MB
<none>                                                      <none>               9954766c4404        3 weeks ago         317MB
<none>                                                      <none>               5471aca149e2        3 weeks ago         317MB
<none>                                                      <none>               de67c74cda48        3 weeks ago         536MB
sonarqube                                                   0.7                 952b5e68f4df        3 weeks ago         536MB
```

Podemos notar que aparecem diversas imagens com a TAG <none> e podendo ocupar espaços consideráveis. Porém alguma dessas imagens sem TAG são imagens usadas por outras imagens e removê-las pode nos fazer perder um certo tempo, mesmo assim é possível removê-las também, mas veremos mais adiante.

Para pesquisarmos somente as imagens que não estão sendo usadas, usamos o comando:<strong> docker images -a --filter "dangling=true" -q --no-trunc</strong>


```shell
╰─$ docker images -a --filter "dangling=true" -q --no-trunc
sha256:031b0be81cf829258f102cb17dd8a07fe9aa06eeb20a6e5e2f67af54532c84f1
sha256:9959c80cfe039e7038fc188b2d0d3f22974d5f280e286a44e3195f3e45e0fa68</pre>
```
Essas imagens podemos ser removidas sem medo, então usaremos o comando abaixo:


```shell
╰─$ docker rmi $(docker images --filter "dangling=true" -q --no-trunc)                                               
Deleted: sha256:031b0be81cf829258f102cb17dd8a07fe9aa06eeb20a6e5e2f67af54532c84f1
Deleted: sha256:9959c80cfe039e7038fc188b2d0d3f22974d5f280e286a44e3195f3e45e0fa68
Deleted: sha256:6de7450e13547144dbb066a9afe8f0e0e2383a5696a9604e366e69ebba9fa3af
Deleted: sha256:c7e984302b4ecf0b9428494665a09bc5c775ebaed5866ce3101a87c786741e29
Deleted: sha256:a8f885350961ed2cb5509033822f01e45e81ec0bed958e80381b0bf5529e14c9
Deleted: sha256:1f192881c9d640e62eeb03f6ed313e256a978c5ffc6e479a19954fcbe80e15b2
Deleted: sha256:f7b55b567ef38e97e7d836939f625e22a412444de9f901ba0c211737cc60a888
Deleted: sha256:355034eb63caebea98f56af94be376d31e29f47f33a50f56a9fb5d8691dd9b8e
Deleted: sha256:56ab38708730dcabc3ac7c6e5e980cf88ea8680110aeacbe1820e0ff27f8348c
Deleted: sha256:8bbd195ad543f77076ecc27a5cde47853add9d94232320ef651ca42225a80702
```

Para remover todas as imagens antigas e não usadas podemos usar o comando abaixo, porém muito cuidado ao usá-lo

```shell
╰─$ docker image prune
```
