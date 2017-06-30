---
title: R因子和表
date: 2017-05-24 20:26:20
tags: R语言
category: R语言
---

0. 因子的创建与修改
```R
> x<-c(5,12,13,12)
> xf<-factor(x)
> xf
[1] 5  12 13 12
Levels: 5 12 13
> str(xf)
 Factor w/ 3 levels "5","12","13": 1 2 3 2
> length(xf)
[1] 4
> xff<-factor(x,levels = c(5,12,13,88))
> xff
[1] 5  12 13 12
Levels: 5 12 13 88
> xff[2]<-88
> xff
[1] 5  88 13 12
Levels: 5 12 13 88
```

0. tapply 根据第二个参数分组,对每个组求平均值.第二个参数可以是列表,
```R
> ages<-c(25,26,55,37,21,42)
> affils<-c("R","D","D","R","U","D")
> tapply(ages,affils,mean)
 D  R  U 
41 31 21 
```

0. split只分组
```R
> split(ages,affils)
$D
[1] 26 55 42

$R
[1] 25 37

$U
[1] 21
```

0. table
```R
> f1<-list(c(5,12,13,12,13,5,13),c("a","bc","a","a","bc","a","a"))
> table(f1)
    f1.2
f1.1 a bc
  5  2  0
  12 1  1
  13 2  1
```










