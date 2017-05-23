---
title: Hexo的markdown添加图片
date: 2017-05-23 21:46:35
tags: Hexo
---

## 操作
1. 设置根目录下`_config.yml`中的两个参数
 * `post_asset_folder: true`
 * `permalink: :year/:month/:day/:title/`
2. 运行该命令`npm install https://github.com/CodeFalling/hexo-asset-image --save`
3. 通过`hexo new "name"`新建文章,此时同时生成一个与文章同名的目录,在此目录中放置图片,例如图片为test.jpg
4. 在markdown中使用![](test.jpg)显示图片

## 插件
* [hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image)

## 测试
![兔斯基](tuzki.gif)
