---
layout: post
title: 3 maneiras de iniciar um cluster Kubernetes localmente
description: '
O Kubernetes se tornou uma das plataformas de orquestração de containers mais utilizada. Existem diversos fatores positivos e negativos no uso do Kubernetes no processo de desenvolvimento, implantação e gerenciamento de aplicações. Kubernetes oferece uma orquestração avançada, permitindo que as aplicações cresçam conforme a demanda. Ele permite também abstrair a infraestrutura, ou seja, se a aplicação roda em um cluster Kubernetes, muito provavelmente roda em qualquer outro cluster de mesma versão. E vem se tornado um padrão dentro das grandes organizações, diminuindo um pouco a complexidade quando um funcionário passa de uma empresa para outra.
'
date: '2024-01-10 10:20:00'
tags: [k8s, kubernetes]
permalink: /3-maneiras-iniciar-cluster-kubernetes-localmente/
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
author: Sidnei Weber
---
<!--more-->

## :anchor: Introdução

O Kubernetes se tornou uma das plataformas de orquestração de containers mais utilizada no mundo. Existem diversos fatores positivos e negativos no uso do Kubernetes no processo de desenvolvimento, implantação e gerenciamento de aplicações.
* Kubernetes oferece uma orquestração avançada, permitindo que as aplicações cresçam conforme a demanda.
* Ele permite também abstrair a infraestrutura, ou seja, se a aplicação roda em um cluster Kubernetes, muito provavelmente roda em qualquer outro cluster de mesma versão.
* E vem se tornado um padrão de mercado dentro das grandes organizações, diminuindo um pouco a complexidade quando um funcionário passa de uma empresa para outra.

## :arrow_forward: Kind

### Sobre
Kind é uma ferramenta para executar clusters locais do Kubernetes usando “nós” de contêiner Docker. Kind foi projetado principalmente para testar o próprio Kubernetes, mas pode ser usado para desenvolvimento local ou CI.

### Instalação
Para usar o kind precisamos já ter o docker instalado. Caso ainda não tenho, pode seguir o tutorial a seguir [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

#### Linux
```shell
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

#### Mac
```shell
# For Intel Macs
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-darwin-amd64
# For M1 / ARM Macs
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-darwin-arm64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind
```

#### Windows
```powershell
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.20.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe
```

### Exemplos
Para criarmos um cluster utilizando o Kind:
```shell
kind create cluster # Criar cluster simples
kind create cluster --name kind-2 # Criar cluster especificando o nome
kind get clusters # Listar clusters
```

Para deletarmos um cluster:

```shell
kind delete cluster
```

Para criarmos um cluster com mais de um node, podemos criar um arquivo de configuração e utilizá-lo como parâmetro. O arquivo pode ser parecido com o abaixo, onde definimos um node como controle plane e dois nodes como workers:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```
Com o arquivo criado, podemos executar o comando:
```shell
kind create cluster --config kind-example-config.yaml
```

## :arrow_forward: K3d

### Sobre

k3d é um wrapper leve para executar k3s (distribuição mínima do Kubernetes do Rancher Lab) no docker. O k3d torna muito fácil a criação de clusters k3s de nó único e de vários nós no docker, por exemplo, para o desenvolvimento local no Kubernetes.
Nota: k3d é um projeto conduzido pela comunidade, mas não é um produto oficial do Rancher (SUSE).

### Instalação

Para o k3d também precisamos ter o docker instalado previamente.

#### Linux
```shell
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

#### Windows
```powershell
choco install k3d
```

#### Mac
```shell
brew install k3d
```

### Exemplo

Criando cluster com k3d:
```shell
k3d cluster create mycluster
kubectl get nodes
```

Assim como no kind, também podemos criar clusters a partir de um arquivo de configuração. Segue um exemplo onde criaremos também um cluster com um node para control plane e dois nodes workers:

```yaml
apiVersion: k3d.io/v1alpha5
kind: Simple
servers: 1
agents: 2
```
E para executar a criação utilizando o arquivo:

```shell
k3d cluster create --config example-config.yaml
```

## :arrow_forward: Minikube

### Sobre

O Minikube configura rapidamente um cluster Kubernetes local no macOS, Linux e Windows.
Acredito que o Minikube seja a opção mais conhecida e utilizada até hoje. Um dois maiores diferenciais do Minikube é poder criar o cluster usando diversos provisionadores. Além do docker como as outras ferramentas, também podemos utilizar [Qemu](https://www.qemu.org), [VirtualBox](https://www.virtualbox.org), dentre outros.

### Instalação

Para realizar a instalação no Linux, utilizaremos os comandos abaixo:
```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

No Mac, o procedimento de instalação é parecido com o do Linux:
```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

Para instalação no Windows, poderemos utilizar o [choco](https://sidneiweber.com.br/instalando-e-gerenciando-programas-no-windows-com-chocolatey/):
```powershell
choco install minikube
```

### Exemplo

Para criar um cluster simples podemos utilizar os comandos abaixo:
```shell
minikube start
minikube kubectl -- get po -A
minikube stop
minikube delete --all
```

Para criar um cluster personalizado, com mais recursos temos diversas opções de comando. Podemos personalizar cpu, memória e outras opções. Todas as opções podem ser conferidas com o comando `minikube start --help`
```
minikube start --cpus=4 --memory=8g --addons=ingress
minikube start --help
```

## :white_check_mark: Conclusão

Essas são algumas das opções para criarmos cluster localmente em nossos computadores. Mas a lista de opções é maior e pode ser explorada caso você não esteja satisfeito com estas opções. O importante é que não faltam alternativas para nos mantermos produtivos, pois podemos usar essas ferramentas para estudos, testes e até mesmo para trabalho.