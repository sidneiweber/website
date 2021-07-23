---
layout: post
title: Introdução Firewall Iptables - Começando
#description: Introdução Firewall Iptables - Começando
date: '2017-02-15 10:44:41 -0300'
author: Sidnei Weber
cover: "/img/uploads/2017/02/Selecao_001.png"
tags:
- iptables
- segurança
- linux
---

### Conceito de Firewall:

O firewall é usado basicamente como um meio de proteção. Dividindo a rede que se pretende deixar segura da rede não segura.  
Geralmente um firewall é instalado na borta da rede, sendo a entrada e saida dos pacotes da mesma, fazendo a leitura de cada pacote e fazendo o controle do que pode passar para rede interna, ou dando o  redirecinamento correto, servindo de filtro.

![iptables](/img/uploads/2017/02/firewall1.png)

O iptables é a ferramenta de firewall a nivel de pacotes do linux desde o kernel 2.4 substituindo o ipchains.  
Ele se baseia nas regras e parametros passados para fazer a filtragem dos pacotes, ou seja, compara as regras com os pacotes.

Para termos uma segurança maior, incluindo um controle de navegação na rede interna, uma dupla muito usada e que combina muito bem, é a dupla Iptables + Squid. Squid é um proxy de navegação, mas essa solução falaremos em outra oportunidade

### Tabelas:

Tabelas são os locais usados para armazenar as chains e conjunto de regras com uma determinada característica em comum. As tabelas podem ser referenciadas com a opção -t tabela e existem basicamente 4 tabelas disponíveis no iptables:  
**Tabela FILTER**: possui cadeias INPUT, OUTPUT, FORWARD  
**Tabela NAT**: possui cadeias PREROUTING, OUTPUT, POSTROUTING  
**Tabela MANGLE**: cadeias PREROUTING, OUTPUT, POSTROUTING, INPUT, FORWARD  
**Tabela RAW**: cadeias PREROUTING, OUTPUT

### O que são chains?

As Chains são locais onde as regras do firewall definidas pelo usuário são armazenadas para operação do firewall.

**INPUT**: aplica regra aos pacotes que chegam ao servidor

**OUTPUT**: aplica regras aos pacotes de rede originados e que partem do servidor

**FORWARD**: aplica regras aos pacotes de rede roteados atraves do servidor (para outro servidor ou outra interface do mesmo servidor)

**PREROUTING**: altera pacotes de rede na hora que chegam e antes do roteamento

**POSTROUTING**: altera pacotes de rede após o roteamento. Usado para SNAT

### Politicas, ações (targets):

**ACCEPT**: pacote permitido  
**DROP**: descartar pacote  
**QUEUE**: enviar o pacote ao userspace (codigo fora do kernel)  
**RETURN**: descontinuar o processamento do pacote e aplicar a regra padrao a ele  
**REJECT**: Descarta o pacote e envia feedback ao remetente  
**DNAT**: Reescreve endereço de destino (NAT)  
**SNAT**: Reescreve endereço de origem (NAT)  
**LOG**: coloca no log informações sobre o pacte

### Estrutura do comando:

#### Comando principal:

iptables subcomando chain parametro1 valor1 parametron valorn ação

#### Subcomandos:

-A cadeia &#8211; anexa a regra ao final da cadeia  
-L [cadeia] lista as regras da cadeia, ou todas caso a cadeia nao seja especificada  
-F [cadeia] apaga todas as regras na cadeia  
-N cadeia &#8211; Lista todas as regras na cadeia  
-P cadeia politica &#8211; configura a regra padrão da cadeia  
-D cadeia linha &#8211; apaga uma regra em um posição na cadeia  
-X [cadeia] excluiu uma cadeia vazia  
-I cadeia linha &#8211; insere uma regra em uma posição na cadeia  
-Z zera os contadores para todas as cadeias

#### Parametros, alguns:

-t tabela (filter é a padrao)  
-j ação  
-p protocolo (especifica o protocolo, icmp, tcp, udp, all)  
-s IP (IP de origem do pacote)  
-d IP (IP de destino do pacte)  
-i interface (nome da interface de rede de entrada do pacote)  
-o interface (nome da interface de rede de saida do pacote)  
&#8211;sport portas (Portas de origem)  
&#8211;dport portas (Portas de destino)  
&#8211;syn (identifica nova requisição de conexao)  
&#8211;icmp-type (tipo de mensagem icmp)

### Checagem de estado dos pacotes (state match):

-m state &#8211;state OPCAO  
**NEW** cria uma nova conexao  
**ESTABLISHED** pacote que pertence a uma conexao existente  
**RELATED** pacote relacionado mas que nao faz parte de uma conexao existente  
**INVALID** pacote nao pode ser identificado (ex. falta memoria, erro ICMP de conexao nao conhecida)

### Extensões:

Extensao TCP  
&#8211;tcp-flags (ALL, SYN, ACK, FIN, etc)  
&#8211;source-port ou &#8211;sport  
&#8211;destination-port ou &#8211;dport

Extensao UDP  
Mesmas opções do TCP

Extensao ICMP  
&#8211;icmp-type

Outras extensoes  
-m limit quando usado em LOG serve para limitar o numero de pacotes escritos durante um certo ponto  
&#8211;limit valor

### Arquivos de logs criados pelo iptables:

Todo tráfego que for registrado pelo iptables é registrado por padrão no arquivo /var/log/kern.log.

### Exemplos:

Exibir todas as regras:

```shell
iptables -L -n -v
```

Verificar regras (padrão filter &#8211;line-numbers (Exibe linhas))

```shell
iptables -L
iptables -S (lista comandos do iptables)
```

Verificar regras tabela nat

```shell
iptables -t nat -L
```

Criando uma regra, DROP em tudo que vai pra porta 123

```shell
iptabels -A INPUT -p tcp --dport 123 -j DROP
```

Inserindo uma regra, -I por padrao insere a regra no final  
\# indicando a linha ja a regra para a posição referente

```shell
iptables -I INPUT 1 -p tcp --sport 321 --dport 123 -J ACCEPT
```

Substituindo uma regra, parametro -R  
\# Substitui a regra 1 (no caso do nosso exemplo)

```shell
iptables -R INPUT 1 -p tcp --sport 321 --dport 123 -J ACCEPT
```

Deletar regra, parametro -D  
\# remove linha 1

```shell
iptables -D INPUT 1
```

\# apagar usando sintaxe, usar mesmo sintaxe com parametro -D

```shell
iptables -D INPUT -p tcp --dport 80 -j ACCEPT
```

Bloquear porta específica  
Sainte:

```shell
iptables -A OUTPUT -p tcp --dport xxx -j DROP
```

Entrante:

```shell
iptables -A INPUT -p tcp --dport xxx -j ACCEPT
```

Ou múltiplas portas:

```shell
iptables -A INPUT  -p tcp -m multiport --dports 22,80,443 -j ACCEPT
iptables -A OUTPUT -p tcp -m multiport --sports 22,80,443 -j ACCEPT
```

Manter registros de Log’s de pacotes bloqueados:

```shell
iptables -A INPUT -i eth0 -j LOG --log-prefix "Quantidade pacotes bloqueados:"
iptables -A INPUT -p tcp -dport 21 -j LOG -log-prefix “Serviço: ftp”
```

Os log’s são salvos em /var/log/messages.

Compartilhar internet, parametro ip\_forward precisa ser 1, geralemte altera-se no arquivo &#8220;/proc/sys/net/ipv4/ip\_forward&#8221;. Lembrando que a mudança não é permanente, a cada reinicialização deve-se alterar o parâmetro novamente ou quando criar um sript de firewall incluir o comando para alterar o arquivo com o cat.

```shell
iptables -A INPUT -m state --state RELATED.ESTABLISHED -j ACCEPT
iptables -A FORWARD -m state --state RELATED.ESTABLISHED -j ACCEPT
```

Ip Masquarade

```shell
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Encaminhamento de portas (Ip forwarding)  
Tentando acessar pela interface wan

```shell
iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j DNAT --to 192.168.10.10:80
iptables -A FORWARD -p tcp -d 192.168.10.10 --dport 80 -j ACCPET
```

Reject  
Descarta pacote e envia um retorno a quem enviou o pacote.

```shell
iptables -A INPUT -s 192.168.10.2 -j REJECT --reject-with icmp-net-unreachable
iptables -A INPUT -s 192.168.10.2 -j REJECT --reject-with icmp-host-unreachable
iptables -A INPUT -s 192.168.10.2 -j REJECT --reject-with icmp-proto-unreachable
```

entre outras opções do &#8211;reject-with

Criar chain

```shell
iptables -t filter -N [chain]
```

Limpar regras

```shell
iptables -t filter -F [CHAIN]
```

remover  Chain criada pelo usuario, parametro -X  
zerar contador, parametro -Z

Alterar regra padrão, o ideal INPUT ser drop por padrão

```shell
iptables -P INPUT DROP
```

Liberar servidor web

```shell
iptables -A INPUT -m state --state new -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -m state --state new -p tcp --dport 443 -j ACCEPT
```

Liberar host especifico

```shell
iptabels -A INPUT -s 192.168.1.20 -j ACCEPT
```

Liberar ping (resposta liberada se output esta ACCEPT tbm)

```shell
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```

Liberar respostas de ping para outras maquinas

```shell
iptables -A INPUT -p icmp --icmp-type echo-reply -j ACCEPT
```

Proteção contra Syn-flood:

```shell
iptables -A FORWARD -p tcp --syn -m limit --limit 1/s -j ACCEPT
```

Port scanner suspeito:

```shell
iptables -A FORWARD -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 1/s -j ACCEPT
```

Ping da morte:

```shell
iptables -A FORWARD -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT
```

Salvando e restaurando as regras:

```shell
iptables-save > arquivo
iptables-restore < arquivo
```
