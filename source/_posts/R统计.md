---
title: R统计
date: 2017-05-26 10:36:43
tags: R语言
---

0. addmargins() 每行的和与每列的和
```R
> a
     [,1] [,2]
[1,]    1    1
[2,]   -1    1
> addmargins(a)
     [,1] [,2] [,3]
[1,]    1    1    2
[2,]   -1    1    0
[3,]    0    2    2
```

0. aggergate() 根据分组应用函数
```R
# Splits the data into subsets, 
# computes summary statistics for each, 
# and returns the result in a convenient form.
aggregate(被分组的数据框,分组的条件,分组后应用的函数)
如 aggregate(aba[,-1],list(aba$Gender),median)
```

0. cut(a,b,labels=FALSE) 把a中的元素根据b划分的区间来分组,返回a被分到b的哪个区间
```R
> z<-runif(1)
> m<-seq(from=0.0,to=1.0,by=0.1)
> cut(z,m,labels = F)
 [1] 10  5  4  5  5  7  7  6  9  8
```

0. 排序
```R
> sort(z) # 根据decreasing = F 设置升序排列
 [1] 0.3923360 0.4462011 0.4541803 0.4978729 0.5658257
 [6] 0.6684959 0.6897590 0.7623066 0.8501145 0.9762984
> order(z)
 [1]  3  4  2  5  8  6  7 10  9  1
> z[order(z)] # 可以根据数据框中的某一列排序
 [1] 0.3923360 0.4462011 0.4541803 0.4978729 0.5658257
 [6] 0.6684959 0.6897590 0.7623066 0.8501145 0.9762984
```

