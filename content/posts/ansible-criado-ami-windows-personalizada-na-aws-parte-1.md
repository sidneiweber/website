---
layout: post
title: 'Ansible: Criado AMI Windows personalizada na AWS (Parte 1)'
subtitle: Configurando Windows para acesso com Ansible
description: Quando vamos trabalhar com Ansbile usando Windows na AWS notamos que as imagens padrões do Windows não estão com o WinRM configurado e as senhas são geradas aleatoriamente usando a chave selecionada, sendo somente acessíveis alguns minutos após a instância iniciar. [Conectando em uma instância Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html).
date: '2019-11-27 09:13:21'
tags: [aws, ansible, windows]
author: Sidnei Weber
#cover: "/img/aws-ami/ansible-aws.png"
---

Quando vamos trabalhar com Ansbile usando Windows na AWS notamos que as imagens padrões do Windows não estão com o WinRM configurado e as senhas são geradas aleatoriamente usando a chave selecionada, sendo somente acessíveis alguns minutos após a instância iniciar. [Conectando em uma instância Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html).

Uma alternativa é criar uma AMI personalizada com WinRM configurado e uma senha pré-definida, estando assim disponível imediatamente para uso. O primeiro passo é iniciar uma instância Windows colocando o script abaixo em User Data. Atenção para o campo onde está definido a senha, o script também irá baixar e executar o script para configurar o WinRM.

```
<powershell>
$admin = [adsi]("WinNT://./administrator, user")
$admin.PSBase.Invoke("SetPassword", "myTempPassword123!")
Invoke-Expression ((New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1'))
</powershell>
```

![UserData](/img/aws-ami/user-data.png)

Caso você use o [Packer](https://www.packer.io) para gerar suas imagens esse script também por ser usado no parâmetro **user_data_file**.

Na configuração de **Security Group** da instância lembre-se de liberar as portas 3389 e 5986. Caso não saiba a porta 5986 é usada para conexão segura com o WinRM, que é usado pelo Ansible para conexão. Após alguns instantes após iniciar a instância ela já vai estar liberada para o acesso.

No arquivo de hosts do ansible, configure usando os dados abaixo e a senha definida no script acima

```yaml
[windows]
ip-host-windows

[windows:vars]
ansible_connection=winrm
ansible_ssh_port=5986
ansible_ssh_user=Administrator
ansible_ssh_pass=myTempPassword123!
```

Após alguns instantes estando a instância disponível para acesso. Podemos testar usando o módulo do ansible [win_ping](https://docs.ansible.com/ansible/latest/modules/win_ping_module.html). No nosso exemplo usando o comando *ansible windows -i hosts -m win_ping*. O retorno deve ser parecido com esse:

![Win Ping](/img/aws-ami/win-ping.png) 

Bom essa é a primeira etapa, na parte 2 iremos ver como configurar o Windows e gerar uma nova AMI personalizada usando o Ansible
