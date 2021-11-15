---
title: Personal Settings
date: 2021-01-25
tag: TCS
category: TCS
mathjax: false
---
Personal Settings. Such as zshrc/vimrc.
<!-- more -->
### zshrc
```
alias c="clear"

alias up="sudo pacman -Syu"
alias ins="sudo pacman -S"
alias uins="sudo pacman -R"

alias re=reboot
alias po=poweroff

```

### vimrc
```
set number
syntax on
set cursorline

set nocompatible

set showmode
set showcmd


filetype on
filetype indent on
filetype plugin on

set autoindent smartindent

set tabstop=4
set expandtab
set shiftwidth=4
set laststatus=2

set ruler
set showmatch

set noerrorbells



```