---
title: Linux清除已删除程序的配置文件
date: 2017-04-17 21:33:14
tags: 命令
category: Linux
---
* 代码
`dpkg -l |grep "^rc"|awk '{print $2}' |xargs aptitude -y purge`


* 解释
1. `dpkg -l` 列出所有安装的软件
2. `grep "^rc"` 找出rc开头的软件(已经被卸载)
3. `awk '{print $2}'` 提取出字符串的第二部分(软件名)
4. `xargs` 将前面产生的结果当成下个函数的参数
5. `aptitude -y purge` 删除软件,过程中出现的选项输入'y'
