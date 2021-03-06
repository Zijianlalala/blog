---
title: R概率
date: 2017-05-25 08:31:38
tags: R语言
category: R语言
---

0. 函数
```R
mean(x) # 平均值
median(x) # 中位数
sd(x) # 标准差
var(x) # 每两列间的协方差
cor(x,y) # 相关系数矩阵
cov(x,y) # 协方差矩阵
choose(n,k) # 计算组合数
combn(items,k) # 在items中取出k项的组合
```

0. 分布
```R
dnorm # 正态概率密度
pnorm # 正态分布函数
qnorm # 正态分位数函数
rnorm # 正态分布的随机数

binom # 二项分布
geom #几何分布
hyper # 超几何分布
nbinom # 负二项分布
pois # 泊松分布

beta #贝塔分布
cauchy # 柯西分布
chisq # 卡方分布
exp # 指数分布
f # F分布
gamma # 伽玛分布
lnorm # 对数正态分布
logis # 逻辑分布
norm # 正态分布
t # t分布
unif # 均匀分布
weibull # 威布尔分布
wilcox # Wilcoxon分布
```

0. 生成随机数,分布前加'r'
```R
如 runif
set.seed() # 可再生随机数
```

0. 分布函数,分布前加'p'
```R
如 pbinom
s<-seq(from=-3,to=+3,length.out = 100) # 生成横坐标
y<-pnorm(x)
plot(x,y,main="Standard Normal Distribution",type='l')
```
 ![标准正态分布分布函数](pnorm.png) 
0. 密度函数,分布前加'd'
```R
如 dgamma
y<-dnorm(x)
plot(x,y,main="Standard Normal Distribution",type='l')
```
 ![标准正态分布密度函数](dnorm.png)

0. 分位数(不懂),分布前加q
```R
如 qnorm
```

0. 生成随机样本
```R
sample(vec,n)
```

0. 生成随机序列
```R
sample(c(T,F),10,replace=TRUE)
```
