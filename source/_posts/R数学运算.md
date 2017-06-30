---
title: R数学运算
date: 2017-05-26 11:11:46
tags: R语言
category: R语言
---

0. 集合运算
```R
union(x,y) # 并
intersect(x,y) # 交
setdiff(x,y) # 差
setequal(x,y) # 检验相等
c %in% y # 检验c是y的元素
choose(n,k) # n个中选k个元素子集的个数,排列
combn(c,n) # 显示集合c中 n个元素的组合
```

0. solve 解线性方程组
```R
> a<-matrix(c(1,-1,1,1),nrow=2)
> b<-c(2,4)
> solve(a,b)
[1] -1  3
```

0. 其他线性代数运算函数
```R
t() # 转置
qr() # QR分解(不懂)
chol() # Cholesky(同不懂)
det() # 行列式的值
eigen() # 矩阵的特征值和特征向量
diag () # 从方阵中提出对角矩阵
sweep() # 数值分析批量运算符
```

0. 其他包
```R
deSolve处理微分方程
```


