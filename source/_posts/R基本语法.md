---
title: R基本语法
date: 2017-05-23 21:22:02
tags: R语言
---

0. 求模运算 %%
```R
> 1%%2
[1] 1
> 2%%2
[1] 0
> 3%%2
[1] 1
```

0. 求商,取整 %/%
```R
> 3%/%2
[1] 1
```
0. hist绘制直方图
```R
> hn<-hist(Nile)
> print(hn)
$breaks
 [1]  400  500  600  700  800  900 1000 1100 1200 1300
[11] 1400

$counts
 [1]  1  0  5 20 25 19 12 11  6  1

$density
 [1] 0.0001 0.0000 0.0005 0.0020 0.0025 0.0019 0.0012
 [8] 0.0011 0.0006 0.0001

$mids
 [1]  450  550  650  750  850  950 1050 1150 1250 1350

$xname
[1] "Nile"

$equidist
[1] TRUE

attr(,"class")
[1] "histogram"
```

0. summary
```R
> summary(hn)
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
























