---
title: vimrc配置(个人备用)
date: 2017-04-17 21:23:11
tags: vim
category: 配置文件
---

## 内容
* ~/.vimrc是vim的配置文件,保存起来用以恢复vim配置
```txt
" 设置编码字符集
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

" 光标移动到buffer的顶部和底部时保持5行距离
set scrolloff=5

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

" YCM配置
" 错误提示不显示左边>>
let g:ycm_enable_diagnostic_signs = 0

let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
```
