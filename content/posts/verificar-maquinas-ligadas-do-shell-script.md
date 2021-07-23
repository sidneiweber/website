---
title: Verificar máquinas ligadas com shell script
date: 2013-01-28T22:58:55-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
  - rede
---
Estava interessado em criar uma forma de scannear a rede para saber quantos micros estavam conectados a ela, saber nome, grupo, MAC ADDRESS.

Para que o script funcione será preciso instalar alguns softwares:

dialog:

```shell
apt-get install dialog
```

nmap:

```shell
apt-get install nmap
```

Copie o código e cole em um arquivo texto e salve com um nome de sua preferência, de permissão para executar (chmod +x nome\_do\_arquivo.sh) e em seguida execute-o (./nome\_do\_arquivo.sh):

```bash
#!/bin/bash

Principal ( ) { ## Inicio Primeiro Bloco
clear
# Loop que mostra o menu principal
while : ; do # Mostra o menu na tela, com as ações disponíveis
opcao=$(
dialog --stdout
--title 'Menu principal'
--menu 'Escolha as opções:'
0 0 0
1 'Scannear IPs da Rede'
0 'Sair')

[ $? -ne 0 ] && Sair # Se apertado CANCELAR ou ESC, então vamos Sair...

case $opcao in ## ## Inicio case
1)ipscan ;;
0)Sair ;;

esac ## Fim da verificação
done
}
## FIM MENU PRINCIPAL

## ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ ##

ipscan ( ) {
clear
inicio=$(dialog --stdout --title 'Firerock ipscan' --inputbox 'Inicio (Ex.192.168.0.1)' 0 0 )

[ $? -ne 0 ] && Principal # Se apertado CANCELAR ou ESC, então vamos Sair...
final=$(dialog --stdout --title 'Firerock ipscan' --inputbox 'Final (Ex.192.168.0.254)' 0 0 )

[ $? -ne 0 ] && ipscan # Se apertado CANCELAR ou ESC, então vamos Sair...

#faixa_rede=`echo "$inicio" | cut -d '.' -f 1-3`
inicio_host=`echo "$inicio" | cut -d '.' -f 4`
final_host=`echo "$final" | cut -d '.' -f 4`

inicio_host=$(expr $inicio_host - 1)
hosts_verificados=$(expr $final_host - $inicio_host)

dialog --yesno 'O processo pode levar alguns minutos! Deseja Continuar?' 6 45

if [ $? = 0 ]; then
continua

else
clear
ipscan
fi
}
continua ( ) {
echo " "
echo "STATUS ENDEREÇO IPv4 MAC ADDRESS NOME GRUPO " &gt; /tmp/hosts.txt
echo "------------------------------------------------------------------------------------------" &gt;&gt; /tmp/hosts.txt

ips_on=$(nmap -sP "$inicio-$final_host" | awk '/^Host/ {print $2}')
online="0"
offline="0"

for ip in $ips_on; do
$(nmblookup -A $ip &gt; /tmp/nm_hosts.txt) ## Saída do comando é # entre clientes Windows e Unix (Linux)
mac_address_win2003=$(cat /tmp/nm_hosts.txt | awk 'NR==9 {print $4}')
mac_address_xp=$(cat /tmp/nm_hosts.txt | awk 'NR==7 {print $4}')
nm_pc=$(cat /tmp/nm_hosts.txt | awk 'NR==2 {print $1}')
nm_grupo=$(cat /tmp/nm_hosts.txt | awk 'NR==3 {print $1}')

mac_address_unix=$(cat /tmp/nm_hosts.txt | awk 'NR==10 {print $4}') ##inicio de verificação p/ ver se é cliente Unix

if [ "$mac_address_unix" == "00-00-00-00-00-00" ]; then ## Se $mac_address_unix for igual a 00-00-00-00-00-00 é um cliente unix
#mac_address=$(cat /tmp/nm_hosts.txt | awk 'NR==10 {print $4}')
nm_pc=$(cat /tmp/nm_hosts.txt | awk 'NR==2 {print $1}')
nm_grupo=$(cat /tmp/nm_hosts.txt | awk 'NR==6 {print $1}')
echo "On $ip $mac_address_unix $nm_pc $nm_grupo" &gt;&gt; /tmp/hosts.txt
online=$(expr $online + 1)

else
if [ "$mac_address_xp" == "" ]; then
echo "On $ip $mac_address_win2003 $nm_pc $nm_grupo" &gt;&gt; /tmp/hosts.txt
online=$(expr $online + 1)

else
if [ ! $nm_pc == 'No' ]; then
echo "On $ip $mac_address_xp $nm_pc $nm_grupo" &gt;&gt; /tmp/hosts.txt
online=$(expr $online + 1)

else
mac_address=
nm_pc=
nm_grupo=
echo "On $ip $mac_address $nm_pc $nm_grupo" &gt;&gt; /tmp/hosts.txt
online=$(expr $online + 1)
fi
fi
fi
done
offline=$(expr $hosts_verificados - $online)
echo "------------------------------------------------------------------------------------------" &gt;&gt; /tmp/hosts.txt
echo " " &gt;&gt; /tmp/hosts.txt

echo "Informações:" &gt;&gt; /tmp/hosts.txt
echo "Faixa de IPs verificados:" &gt;&gt; /tmp/hosts.txt
echo "Inicio: $inicio" &gt;&gt; /tmp/hosts.txt
echo "Final : $final" &gt;&gt; /tmp/hosts.txt
echo " " &gt;&gt; /tmp/hosts.txt
echo "Hosts Verificado: $hosts_verificados" &gt;&gt; /tmp/hosts.txt
echo "Hosts Online: $online" &gt;&gt; /tmp/hosts.txt
echo "Hosts Offline: $offline" &gt;&gt; /tmp/hosts.txt
echo " " &gt;&gt; /tmp/hosts.txt
echo "OBS:" &gt;&gt; /tmp/hosts.txt
echo "Endereços com status On que não possuem (ou é invalido) MAC ADDRESS, NOME, GRUPO ou qualquer um, geralmente são clientes UNIX." &gt;&gt; /tmp/hosts.txt
echo "Exemplos:" &gt;&gt; /tmp/hosts.txt
echo "On 192.168.0.1 " &gt;&gt; /tmp/hosts.txt
echo "On 192.168.0.254 00-00-00-00-00-00 Meupc MEUGRUPO" &gt;&gt; /tmp/hosts.txt
dialog
--title 'Lista de Hosts Online'
--textbox /tmp/hosts.txt
0 0

ipscan
}

## ------------------------------------------------------------------------------------------------------------------------------------------------------
Sair ( ) {
clear
exit
}
Principal</pre>
```

Fonte: http://www.vivaolinux.com.br/dica/Ipscan-com-shell-script+dialog+nmap