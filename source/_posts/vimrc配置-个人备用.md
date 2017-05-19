---
title: vimrc配置(个人备用)
date: 2017-04-17 21:23:11
tags: Linux
---

## 说明
* ~/.vimrc是vim的配置文件,保存起来用以恢复vim配置


## 内容

" 设置编码字符集,解决某些文件打开乱码的情况 
set encoding=utf-8 fileencodings=ucs-bom,utf-8,cp936

" 显示行号
set number

" 空格代替制表符
set expandtab

" 不适用vi键盘
set nocompatible

" 突出显示当前行
set cursorline

" 自动缩进
set autoindent
set cindent

" Tab键的宽度
set tabstop=4

" 统一缩进为4
set softtabstop=4
set shiftwidth=4

" 自动判断换行格式
set fileformats=unix,dos

" vundle
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()
Bundle 'gmarik/vundle'
Bundle 'bling/vim-airline'
Bundle 'davidhalter/jedi-vim'
Plugin 'Valloric/YouCompleteMe'
set laststatus=2

