---
title: vim配置成ide过程记录
tags: vim
category: 配置文件
date: 2017-07-09 22:04:27
---
```Bash
# 安装基本软件
sudo apt install vim exuberant-ctags git pip3 curl
sudo pip3 install dbgp vim-debug pep8 flake8 pyflakes isort

# 备份.vimrc,该文件在HOME目录
# 下载vimrc,几秒中下载完成
curl -O https://raw.githubusercontent.com/fisadev/fisa-vim-config/master/.vimrc

mv .vimrc ~/.vimrc

# 安装,时间五分钟左右
vim +BundleClean +BundleInstall! +qa

# 改文件所属的用户,不明白为什么要这么做
sudo chown -R $USER ~/.vim/
```
