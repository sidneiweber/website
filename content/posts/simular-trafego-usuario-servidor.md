---
layout: post
title: Simular tráfego de usuário para um servidor
description: '
Quando queremos testar algumas coisas servidor, como teste de carga ou teste de desempenho podemos utilizar essa ferramenta que simula conexões dos usuários.
'
date: '2021-06-09 11:00:00'
tags: [linux]
author: Sidnei Weber
---

Vamos simular o tráfego para um servidor utlizando a ferramenta `ab` que foi criado pelo Apache para testar seu próprio serviço.

```
ab -c 20 -n 100 -m GET http://127.0.0.1/
```

### Onde:
**-c** Número de solicitações enviadas ao mesmo tempo<br>
**-n** Número total de solicitações enviadas para o servidor<br>
**-m** Método HTTP utilizado<br>

Existem diversas outras opções que podem ser encontradas [aqui](https://httpd.apache.org/docs/2.4/programs/ab.html).

Como resposta teremos diversas informações que podem nos ajudar a entender se o servidor está preparado para receber bastante tráfego, se o desempenho seria satisfatório e assim por diante.

```
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient).....done


Server Software:        Apache/2.4.18
Server Hostname:        127.0.0.1
Server Port:            80

Document Path:          /
Document Length:        2817 bytes

Concurrency Level:      20
Time taken for tests:   0.047 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      300800 bytes
HTML transferred:       281700 bytes
Requests per second:    2116.45 [#/sec] (mean)
Time per request:       9.450 [ms] (mean)
Time per request:       0.472 [ms] (mean, across all concurrent requests)
Transfer rate:          6217.06 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       0
Processing:     1    9  11.8      3      34
Waiting:        1    9  11.8      3      34
Total:          1    9  11.9      3      34

Percentage of the requests served within a certain time (ms)
  50%      3
  66%      4
  75%      4
  80%     31
  90%     33
  95%     34
  98%     34
  99%     34
 100%     34 (longest request)
```
