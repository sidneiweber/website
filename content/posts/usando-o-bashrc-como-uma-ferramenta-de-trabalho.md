---
title: Usando o bashrc como uma ferramenta de trabalho
description: Usando o bashrc como uma ferramenta de trabalho
date: 2016-12-16T15:45:29-03:00
author: Sidnei Weber
layout: post
tags:
  - linux
  - shell script
---
Segue o meu .bashrc com vários atalhos e alterações.

```shell
# Cor Verde (Usuario comum)
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
# Cor Vermelha (root)
#PS1='${debian_chroot:+($debian_chroot)}\[\033[1;31m\]\u@\h\[\033[00m\]:\[\033[1;34m\]\w\[\033[00m\]\$ '

# Completion
if [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
fi

# Configurações Historico
export HISTIGNORE="&:ls:[bf]g:exit:reset:clear:cd*"
export HISTSIZE=4096
export HISTCONTROL="ignoreboth:erasedups"
shopt -s histreedit;

# Mans coloridas
export MANPAGER="/usr/bin/most -s"

# Alias
alias update='pacman -Syu'

alias ls='ls --color=auto'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'

# Extração de arquivos
# Uso: extrair < arquivo > 
extrair() { 
if [ -f $1 ] ; then
case $1 in
*.tar.bz2) tar xvjf $1 ;; 
*.tar.gz) tar xvzf $1 ;; 
*.tar.xz) tar xvJf $1 ;; 
*.bz2) bunzip2 $1 ;; 
*.rar) unrar x $1 ;; 
*.gz) gunzip $1 ;; 
*.tar) tar xvf $1 ;; 
*.tbz2) tar xvjf $1 ;; 
*.tgz) tar xvzf $1 ;; 
*.zip) unzip $1 ;; 
*.Z) uncompress $1 ;; 
*.7z) 7z x $1 ;; 
*.xz) unxz $1 ;; 
*.exe) cabextract $1 ;; 
*) echo "\`$1': unrecognized file compression" ;; 
esac 
else
echo "\`$1' is not a valid file"
fi
}

# Misc
alias musica='mocp -m /media/Documentos/Musica/'

# Top 10 ( mostra os 10 comandos mais utilizados ). copyright 2007 - 2010 Christopher Bratusek 
function top10() { 
history | awk '{a[$2]++ } END{for(i in a){print a[i] " " i}}' | sort -rn | head
} 

# alisa previsao tempo
alias tempo='curl http://wttr.in/sapiranga'
```