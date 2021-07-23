---
layout: post
title: Exemplo de comandos da AWS S3 para gerenciar buckets
subtitle: Comandos para gerenciar Bucket e Objetos no S3 da AWS
description: Este tutorial explica os conceitos básicos de como gerenciar buckets do S3 e seus objetos usando o aws s3 cli usando os seguintes exemplos
date: '2019-10-25 16:55:50'
tags:
- aws
- s3
#cover: "/img/awss3.png"
author: Sidnei Weber
---

Este tutorial explica os conceitos básicos de como gerenciar buckets do S3 e seus objetos usando o aws s3 cli usando os seguintes exemplos:

## Criar bucket

```bash
aws s3 mb s3://bucketname

# região diferente
aws s3 mb s3://bucketname --region us-east-2
```

## Remover Bucket

```bash
aws s3 rb s3://bucketname
aws s3 rb s3://bucketname --force
```

## Opção ls

```bash
aws s3 ls
aws s3 ls s3://bucketname
aws s3 ls s3://bucketname --recursive
aws s3 ls s3://bucketname --recursive  --human-readable --summarize
```

## Opção cp

```bash
aws s3 cp getdata.php s3://bucketname
aws s3 cp /local/dir/data s3://bucketname --recursive
aws s3 cp s3://bucketname/getdata.php /local/dir/data
aws s3 cp s3://bucketname/ /local/dir/data --recursive
aws s3 cp s3://bucketname/init.xml s3://backup-bucket
aws s3 cp s3://bucketname s3://backup-bucket --recursive
```

## Opção mv

```bash
aws s3 mv source.json s3://bucketname
aws s3 mv s3://bucketname/getdata.php /home/project
aws s3 mv s3://bucketname/source.json s3://backup-bucket
aws s3 mv /local/dir/data s3://bucketname/data --recursive
aws s3 mv s3://bucketname s3://backup-bucket --recursive
```

## Opção rm

```bash
aws s3 rm s3://bucketname/queries.txt
aws s3 rm s3://bucketname --recursive
```

## Opção sync

```bash
aws s3 sync backup s3://bucketname
aws s3 sync s3://bucketname/backup /tmp/backup
aws s3 sync s3://bucketname s3://backup-bucket
```

## Criar website bucket

```bash
aws s3 website s3://bucketname/ --index-document index.html --error-document error.html
```

Caso tenham mais dúvidas, segue a documentação oficial: [https://docs.aws.amazon.com/cli/latest/reference/s3/](https://docs.aws.amazon.com/cli/latest/reference/s3/)
