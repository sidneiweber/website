---
title: Comandos de manipulação de permissões no Linux
date: 2016-12-13T15:54:07-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
---
## Entenda um pouco o sistema de permissões no sistema Linux

Pelo fato do Linux ser multiusuário, as permissões deve estar bem alinhadas para que o usuário tenha acesso somente aquilo que é de seu direito, incluindo pastas, arquivos e inclusive periféricos, como impressoras e drivers de CD por exemplo.  
A Estrtutura básica das permissões é separada em três classes:

  * Dono: define as permissões ao dono do arquivo, ou seja, que criou o arquivo.
  * Grupo: define as permissões para o grupo de usuários ao qual o dono do arquivo pertence.
  * Outros: define as permissões para os demais usuarios, que não seja o dono ou que faça parte do grupo.

Dentro de cada classe citada anteriormente, temos os tipos de acesso:

  * Leitura (r): permissão de leitura para arquivos. Caso seja um diretório permite a listagem do seu conteúdo.
  * Escrita (w): permite a escrita em arquivos ou criação dentro de pastas.
  * Execução (x): permite a execução de um programa "executável".

Quando a gente executa um ls -la no terminal, podemos ver como funciona:

![ls ><](/img/uploads/2016/12/Selecao_003.png)

No começo de cada linha temos 10 colunas referente as permissões de cada arquivo, onde a primeira coluna identifica o tipo do arquivo, se é um arquivo, um diretório, um link, etc. Após a primeira coluna, temos um conjunto de três colunas para cada classe de permissões, ou seja, do dono, do grupo e outros contando da esquerda para direita.

### Diferença de permissões entre arquivos e diretórios:

| Objeto  | Leitura (r) | Gravação (w) | Execução (x) |
|---|---|---|---|
| Arquivo | Permite ler o conteúdo do arquivo | Permite alterar o arquivo | Permite executar arquivo como um programa |
| Diretório | Permite listar o conteúdo do diretório | Permite criar e apagar arquivos no diretório | Permite ler e gravar arquvos no diretório |

## Lista de comandos para trabalhar as permissões

### chmod

Muda as permissões de acesso a um determinado arquivo ou diretório.

#### Sintaxe

```shell
chmod [opções] [permissões] [diretório/arquivo]
```

#### Opções

_-v, --verbose_

Mostra todos os arquivos que estão sendo processados.

_-f, --silent_

Não mostra a maior parte das mensagens de erro.

_-c, --change_

Semelhante a opção -v, mas só mostra os arquivos que tiveram as permissões alteradas.

_-R, --recursive_

Muda permissões de acesso do _diretório/arquivo_ no diretório atual e sub-diretórios.

ugoa+-=rwxXst

  * _ugoa_ - Indica o nível de acesso será mudado. Especificam, em ordem, usuário (u), grupo (g), outros (o), todos (a).
  * _+-=_ - _+_ adiciona a permissão, _&#8211;_ retira a permissão do arquivo e _=_ define a permissão exatamente como especificado.
  * rwx - _r_ permissão de leitura do arquivo. _w_ permissão de gravação. _x_ permissão de execução (ou acesso a diretórios).

#### Exemplos

```shell
chmod o-r teste.txt
# Retira (-) a permissão de leitura (r) do arquivo teste.txt para os outros usuários (usuários que não são donos e não pertencem ao grupo do arquivo teste.txt).
chmod a+x teste.txt
# Inclui (+) a permissão de execução do arquivo teste.txt para o dono, grupo e outros usuários.
chmod 777 teste.txt
# Coloca permissão total de escrita, leitura e execução no arquivo teste.txt para todos usuários
```

![chmod ><](/img/uploads/2016/12/Selecao_004.png)

### chgrp

Muda o grupo de um arquivo ou diretório

#### Sintaxe

```shell
chgrp [opções] [grupo] [arquivo/diretório]
```

#### Opções

_-v, --verbose_

Mostra todas as mensagens e arquivos sendo modificados.

_-R, --recursive_

Altera os grupos de arquivos/sub-diretórios do diretório atual.

### chown

Altera dono de um arquivo ou diretório. Pode também ser usado para mudar o grupo

#### Sintaxe

```shell
chown [opções] [dono.grupo] [diretório/arquivo]
```

#### Opções

_-v, --verbose_

Mostra os arquivos enquanto são alterados.

_-R, --recursive_

Altera dono e grupo de arquivos no diretório atual e sub-diretórios.

#### Exemplos

```shell
chown sidnei teste.txt
# Muda o dono do arquivo teste.txt para sidnei.

chown sidnei.admin teste.txt
# Muda o dono do arquivo teste.txt para sidnei e seu grupo para admin.
```

### umask

O comando umask define as permissões iniciais quando um arquivo ou diretório for criado. Digitando _umask_ sem nenhum parâmetro podemos visualizar nosso umask atual. O umask cria permissão diferente caso o arquivo seja um executável e se for um arquivo texto. Veja a tabela a seguir:

     ---------------------------------------------
     |       |        ARQUIVO       | DIRETÓRIO  |
     | UMASK |----------------------|            |
     |       |   Binário  |  Texto  |            |
     |------------------------------|------------|
     |   0   |    r-x     |   rw-   |    rwx     |
     |   1   |    r--     |   rw-   |    rw-     |
     |   2   |    r-x     |   r--   |    r-x     |
     |   3   |    r--     |   r--   |    r--     |
     |   4   |    --x     |   -w-   |    -wx     |
     |   5   |    ---     |   -w-   |    -w-     |
     |   6   |    --x     |   ---   |    --x     |
     |   7   |    ---     |   ---   |    ---     |
     ---------------------------------------------

Caso seu umask seja 022, os arquivos de texto será criados por padrão com permissão 644, ou seja, rw- para o dono, r-- para o grupo e r-- para outros. Para que alterar o umask, geralmente deve-se alterar seu valor no arquivo **/etc/profile**.