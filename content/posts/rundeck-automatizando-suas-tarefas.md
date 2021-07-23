---
layout: post
title: Rundeck - Automatizando suas tarefas
date: '2018-04-09 21:08:17 -0300'
author: Sidnei Weber
tags: [devops, rundeck]
---

O que é esse tal de Rundeck

Falando a grosso modo Rundeck é um automatizador e agendador de tarefas. Obviamente que ele faz muito mais do que isso, ele consegue rodar essas tarefas tanto em Linux quanto em Windows. Consegue-se criar um workflow de tarefas, fazendo uma tarefa depender de outra ou conforme sua execução com sucesso ou insucesso.

Escrito em java e tem sua administração por uma interface web bem simples de ser usado e usa ssh para conexões com Linux e Winrm para conexões com Windows. As configurações ficam salvas em arquivos, geralmente XML, tanto dos jobs (trabalhos) quanto dos nodes (hosts).

Tem uma grande vantagem pois não precisa de agentes rodando nas máquinas clientes e se consegue fazer uma verdadeira orquestração de jobs, podendo escolher se os jobs vão rodar em paralelo ou sequência e assim por diante. Se consegue criar chaves de acesso, ou chaves com senhas dentro do próprio dashboard.  
Existe uma integração com diversos serviços:

![rundeck ><](/img/uploads/2018/04/Captura-de-tela-de-2018-04-09-21-09-32-300x142.png)

Uma das poucas desvantagens que achei até o momento, que algumas configurações como de usuários, nodes, terem que ser feitas pela linha de comando editando diretamente os arquivos.

Para usar o Rundeck, precisamos ter instalado o java (1.8) em nosso servidor. Realizei alguns testes no Centos.

```shell
java -version

java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

E instalaremos o rundeck adicionando o repositório.

```shell
rpm -Uvh http://repo.rundeck.org/latest.rpm
yum install rundeck
```

Após instalação ocorrer tudo bem, podemos conferir verificando a porta onde o serviço roda. Lembrando que caso tenha algum firewall ou bloqueio de portas o mesmo deve ser liberado.

```shell
netstat -an | egrep '4440|4443'
tcp46      0      0  *.4440                 *.*                    LISTEN
```

Agora basta acessarmos o IP do nosso servidor seguido da porta. Ex: http://localhost:4440 e usar a famosa senha de admin/admin

![rundeck ><](/img/uploads/2018/04/1DavE3K2jDhlyPljVdaeAeQ-300x300.png)

Bom de momento vamos ficar somente com a instalação, nos próximos artigos vamos aprender a cadastrar nodes, executar comandos, scripts e demais coisas que essa baita ferramenta disponibiliza.

Site: [http://rundeck.org/](http://rundeck.org/)
