---
id: 19
title: Apache restrito httpd.conf
description: Apache restrito httpd.conf
date: 2009-09-28T18:51:29-03:00
author: Sidnei Weber
layout: post
tags:
  - ldap
  - apache
---
Exemplo de como restringir o acesso com apache realizando autenticação no LDAP.

# Diretório Restrito

```bash
Options Indexes FollowSymLinks Includes  
AllowOverride AuthConfig

#Autenticao  
AuthName "Acesso ao SARG"
AuthType Basic  
AuthBasicProvider "ldap"
AuthLDAPURL "ldap://201.7.197.39:389/dc=topazio,dc=com?uid?sub"
authzldapauthoritative Off  
#AuthUserfile /etc/apache2/apache_passwd  
require user sweber  
#AuthUserfile /etc/apache2/apache_passwd  
#require valid-user

Order allow,deny  
Allow from all
```

# Diretorio Install

```bash
Options Indexes FollowSymLinks Includes  
AllowOverride AuthConfig

AuthName "Acesso ao INSTALL"
AuthType Basic  
AuthBasicProvider "ldap"
AuthLDAPURL "ldap://201.7.197.39:389/dc=topazio,dc=com?uid?sub"
authzldapauthoritative Off  
#AuthUserfile /etc/apache2/apache_passwd  
require user sweber

Order allow,deny  
Allow from all
```
