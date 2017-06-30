---
title: R基本语法
date: 2017-05-23 21:22:02
tags: R语言
category: R语言
---

0. 求模运算 %%
```R
1%%2
```

0. 求商,取整 %/%
```R
3%/%2
[1] 1
```
0. hist绘制直方图
```R
hn <- hist(Nile)
```

0. summary
```R
summary(hn)
         Length Class  Mode     
breaks   11     -none- numeric
counts   10     -none- numeric
density  10     -none- numeric
mids     10     -none- numeric
xname     1     -none- character
equidist  1     -none- logical
```
![直方图样例](HistogramOfNile.png)

0. 新建PDF文件,写入直方图,关闭文件,写入硬盘,rnorm(100),正态分布,100个变量
```R
pdf("xh.pdf")
hist(rnorm(100))
dev.off()
```
























