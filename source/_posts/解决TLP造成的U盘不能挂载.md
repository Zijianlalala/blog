---
title: 解决TLP造成的U盘不能挂载
date: 2017-04-21 23:15:41
tags: 工具
---

## 描述
* TLP brings you the benefits of advanced power management for Linux without the need to understand every technical detail. TLP comes with a default configuration already optimized for battery life, so you may just install and forget it. Nevertheless TLP is highly customizable to fulfil your specific requirements.


* most internal laptop bluetooth devices and all external bluetooth dongles are USB devices. Some do not implement autosuspend mode properly, giving trouble to connected devices.


## 解决方法
1. 执行`tlp-stat -u | grep btusb`并记录ID后面的数字，例如ID XXXX:XXXX
2. 管理员权限打开配置文件`sudo vim /etc/default/tlp`
3. 配置文件中添加`USB_BLACKLIST="XXXX:XXXX"`
4. 执行`sudo tlp usb`应用配置

## 参考文献
* [官方网站FAQ](http://linrunner.de/en/tlp/docs/tlp-faq.html#powertop)
