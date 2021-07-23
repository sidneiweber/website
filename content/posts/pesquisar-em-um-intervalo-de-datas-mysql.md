---
title: Pesquisar em um intervalo de datas Mysql
description: Pesquisar em um intervalo de datas Mysql
date: 2015-07-06T15:50:07-03:00
author: Sidnei Weber
layout: post
tags: [mysql]
---

```sql
SELECT * FROM `contas_pagar` WHERE data BETWEEN ('2014-09-01') AND ('2014-09-31');
```

E para fazer uma soma em um intervalo de datas:

```sql
SELECT SUM(valor) as total FROM contas_pagar WHERE valor BETWEEN ('2014-09-01') AND ('2014-09-31');
```