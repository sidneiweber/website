---
id: 813
title: Iniciando com MongoDB
subtitle: Primeitos passos usando o shell do Mongo.
description: Primeitos passos usando o shell do Mongo.
date: 2019-04-02T16:59:07-03:00
author: Sidnei Weber
layout: post
#cover: /img/uploads/2019/03/mongodb-logo-rgb-j6w271g1xn-768x203.jpg
tags: [mongo]
---
Após termos realizado a instalação do MongoDB [nesse post](https://sidneiweber.com.br/2019/03/08/instalando-mongodb-community-edition-4-0-no-ubuntu/), hoje vamos iniciar com os primeiros passos com nosso banco de dados.

### Utilizando o Mongo Shell

Quando estamos utilizando o mongo em localhost (127.0.0.1) não é necessário usuário e senha para realizar a conexão.

```shell
$mongo
MongoDB shell version v4.0.6
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("319f1942-5755-4448-a5a5-282acf77ed6d") }
MongoDB server version: 4.0.6
```

Primeiro comando a ser usado é o **use,** utilizado para selecionar o banco usado.

```shell
$use usuarios
switched to db usuarios
```

Método **insert( )** para armazenar um documento contendo as chaves "name" e "idade". As informações são armazenadas na estrutura, chave/valor, similar ao JSON.

```shell
db.usuarios.insert({ name: "Sidnei", idade: 31} )
WriteResult({ "nInserted" : 1 })
```

No comando db.**usuarios**.insert, esse usuários corresponde a uma coleção, e ela não precisa de uma estrutura definida, podendo ser dinâmica caso haja necessidade. Podemos usar o comando abaixo sem alterar nenhum tipo de definição de estrutura que o insert irá funcionar normalmente.

```shell
db.usuarios.insert({ name: "Sidnei", idade: 31, cidade: "Sapiranga" })
WriteResult({ "nInserted" : 1 })
```

Use o método **find( )** para visualizar os documentos inseridos. Vamos buscar o que foi salvo na coleção **usuarios**:

```shell
db.getCollection('usuarios').find({})
{ "_id" : ObjectId("5ca3bbb7719bb01a6641f7cb"), "name" : "Sidnei", "idade" : 31 }
{ "_id" : ObjectId("5ca3bd99719bb01a6641f7cc"), "name" : "Sidnei", "idade" : 31, "cidade" : "Sapiranga" }
```

Essa pesquisa também pode ser aprofundada, por exemplo pesquisando só quem está na cidade "Sapiranga"

```shell
db.getCollection('usuarios').find({cidade:"Sapiranga"})
{ "_id" : ObjectId("5ca3bd99719bb01a6641f7cc"), "name" : "Sidnei", "idade" : 31, "cidade" : "Sapiranga" }
```

E para sairmos do nosso shell basta executar o comando:

```shell
$exit
bye
```

Para o início está de bom tamanho, até a próxima.
