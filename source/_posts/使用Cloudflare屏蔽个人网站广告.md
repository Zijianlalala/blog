---
title: 使用Cloudflare屏蔽个人网站广告
date: 2017-04-22 18:04:34
tags: 工具
---

## 介绍
* 两个会用到的网站:[Cloudflare官网](https://www.cloudflare.com/),[万网](https://whois.aliyun.com/)
* 因为运营商劫持,原本简洁的网站(Hexo)加上烦人的广告
* 究其原因还是http协议不够完善,改用https可以逃过运营商的广告
* 原理详见[https](http://baike.baidu.com/link?url=Kq6gxU_rHGAGX-kWZww_HDqGaafeyeiwbJz1gWWhOdM9rawhkPRfl2yfidKD4IP54xqHMfEGCwUMj0RucbO7A_)


## 步骤
1. 首先要有一个网站
2. 注册Cloudflare,添加网站域名信息
3. 按照提示修改DNS(必需步骤)
4. 在Cloudflare面板修改相关设置,让网站只能通过https访问

## 注意
* 以上操作修改完大概一个小时才会生效
* 生效后Cloudflare主页显示"Status:Active"
* 会造成某些运营商提供的网络不能访问,比如"山东广电网络"

