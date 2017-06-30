---
title: 对于Hibernate后第二次唤醒出现GRUB菜单的探究
date: 2017-06-12 23:47:33
tags: Linux
category: Linux
---

## 描述
### 环境
* Kernel 4.10.0-generic
* Memory 3.8G
* Swap 4.2G
* GRUB2
* /etc/default/grub
```
GRUB_DEFAULT=0
GRUB_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
```


### 问题
0. Hibernate,第一次正常唤醒
0. 再次Hibernate,第二次唤醒,出现GRUB菜单,且倒计时为30秒

## 解决
### 肤浅的解决办法
* 直接修改/boot/grub/grub.cfg
 0. 给文件添加写权限
 0. 将第86行的timeout=30改为timeout=0

### 稍微明智的解决办法
* 修改/etc/default/grub
 0. 添加`GRUB_RECORDFAIL_TIMEOUT=0`

### 梦想中的解决办法
* 解决将Hibernate判断为异常断电的问题

## 解释
* 源码
```shell
if [ "${recordfail}" = 1 ] ; then
    set timeout=30  
```
* 出现异常断电的时候,recordfail会被设置为1,从异常断电中重启系统,显示grub菜单是个合理的设计,但是不明白,为什么第二次Hibernate会被识别为异常断电
* 直接设置timeout=0,在正常情况下没有问题,一旦系统真的出了问题就是个大问题,这是一种肤浅且不安全的设置方法,但目前为止还没有找到更合适的方法进行设置

## 参考文献
[grub恢复recordfail](http://blog.csdn.net/ppp2006/article/details/42098677)
