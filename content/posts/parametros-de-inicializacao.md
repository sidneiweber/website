---
title: Parâmetros de Inicialização
date: 2009-09-24T20:10:48-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
---
linux apm=off acpi=off noapic nolapic nopcmcia noapci nosmp pnpbios=off nomce &#8211; Se quiser pode substituir  
o correspondente por algum destes aqui: (apm=power-off ou noapm) (pci=noacpi) (apci=off ou pci=noapci)

**APM** &#8211; Advanced Power Management: Esse comando de inicialização do x86 desativa o Gerenciador Avançado de Energia. É útil porque algumas BIOS têm erros no gerenciamento de energia e tendem a travar  
**ACPI** &#8211; Advanced Configuration and Power Interface: Desliga o recurso, responsável pela configuração e gerenciamento de energia no computador. É usado em notebooks e desktops para, por exemplo, colocar o computador em estado de hibernação. Algumas placas simplesmente têm uma implementação furada da ACPI ou precisam de configuração especial pra funcionar corretamente. Outras placas, por outro lado, precisam do parâmetro acpi=force porque têm problemas se o ACPI não estiver ativado.  
**APIC** &#8211; Advanced Programmable Interrupt Controller: É um controlador de interrupções integrado no processador. Esse comando de inicialização do x86 diz ao kernel para não utilizar o chip APIC. Pode ser útil para algumas placas-mãe com um APIC danificado (como o Abit BP6) ou com um BIOS cheio de erros. Sistemas baseados nos chips nForce3 da NVIDIA (como o ASUS SK8N) foram conhecidos por caírem durante a detecção do IDE no momento da inicialização ou por apresentarem outros problemas de interrupção.  
**PCMCIA** &#8211; Esse comando ignora qualquer controlador PCMCIA no sistema, que geralmente são de notebook/laptop pelo que sei  
**NOSMP** &#8211; Desativa o suporte da placa mãe a multi-processamento e hyperthreading. Algumas placas sequer têm um segundo processador, mas são esquizofrênicas, acreditam que têm e reclamam bem alto se você não concordar com  
elas.  
**PNPBIOS=OFF** &#8211; Desliga o recurso plug-and-play do barramento ISA. Isso resolve nos casos em que a placa-mãe acha que é uma boa idéia reservar um monte de interrupções para dispositivos não existentes ou interrupções que  
deveriam ficar com dispositivos on-board no barramento PCI.