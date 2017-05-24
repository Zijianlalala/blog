---
title: R向量
date: 2017-05-24 09:09:26
tags: R语言
---

0. all(c),any(c),typeof(c)
```R
> x<-1:10
> y<-x>8
> all(y)
[1] FALSE
> any(y)
[1] TRUE
```
0. 判断两个向量相等,建议使用ALL(a==b);identical(a,b)判断相等,同时判断数据类型
```R
> x<-1:2
> y<-c(1,2)
> all(x==y)
[1] TRUE
> identical(x,y)
[1] FALSE
> typeof(x)
[1] "integer"
> typeof(y)
[1] "double"
> x
[1] 1 2
```

0. 设置名字
```R
> x<-1:2
> names(x)<-c("a","b")
> x
a b 
1 2 
```

0. 长度length(c)和样式mode(c)
```R
> x<-c(5,12,13)
> length(x)
[1] 3
> mode(x)
[1] "numeric"
> 
> x<-c("abc")
> length(x)
[1] 1
> mode(x)
[1] "character"
> 
> x<-c('a','b')
> length(x)
[1] 2
> mode(x)
[1] "character"
```

0. ifelse(a,b,c)语句,类似c++中的条件表达式a?b:c
```R
> x<-1:10
> y<-ifelse(x%%2,5,12)
> y
 [1]  5 12  5 12  5 12  5 12  5 12
```

0. 带默认值的函数,布尔值建议用TRUE/FALSE,而不是T/F
```R
> g <- function(x,y,z=T){
+     if(z)
+         return(x)
+     else
+         return(y)
+ }
> g(1,2,TRUE)
[1] 1
> g(1,2,F)
[1] 2
```

0. 向量vector按行结合rbind(c1,c2),按列结合cbind(c1,c2)
```R
> a<-c(1,2)
> b<-c(3,4)
> m<-rbind(a,b)
> n<-cbind(a,b)
> m
  [,1] [,2]
a    1    2
b    3    4
> n
     a b
[1,] 1 3
[2,] 2 4
> m[1,2]
a 
2 
> n[1,2]
b 
3 
```

0. 均值mean(c)和标准差sd(c)
```R
> x<-c(1,2,3)
> x[2:3]
[1] 2 3
> mean(x)
[1] 2
> sd(x)
[1] 1
```

0. sapply(c,f),将函数应用于整个向量,第一行输出原始数据,第二行输出运算结果
```R
> ans = function(z) return (c(z,z^2))
> x<-1:8
> ans(x)
 [1]  1  2  3  4  5  6  7  8  1  4  9 16 25 36 49 64
> matrix(ans(x),ncol=2)
     [,1] [,2]
[1,]    1    1
[2,]    2    4
[3,]    3    9
[4,]    4   16
[5,]    5   25
[6,]    6   36
[7,]    7   49
[8,]    8   64
> 
> sapply(1:8,ans)
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
[1,]    1    2    3    4    5    6    7    8
[2,]    1    4    9   16   25   36   49   64
```


0. NA和NULL
```R
> x<-c(88,NA,12,168,13)
> x
[1]  88  NA  12 168  13
> x[x>50]
[1]  88  NA 168
> 
> mean(x)
[1] NA
> mean(x,na.rm = TRUE)
[1] 70.25
> 
> x<-c(88,NULL,12,168,13)
> mean(x)
[1] 70.25
> 
> z<-NULL
> for(i in 1:10) if(!i%%2) z<-c(z,i)
> z
[1]  2  4  6  8 10
```

0. nchar(字符个数),toupper,tolower
```R
> x<-"abcdaebfab"
> nchar(x)
[1] 10
> y<-toupper(x)
> y
[1] "ABCDAEBFAB"
```

0. 字符串替换
```R
> # a->x , b->y
> chartr("ab","xy",x)
[1] "xycdxeyfxy"
> # ab->xy ,但只替换地一个
> sub("ab","xy",x)
[1] "xycdaebfab"
> # ab->xy ,替换所有
> gsub("ab","xy",x)
[1] "xycdaebfxy"
> 
> rep(x,times=3,each=2,length.out=2)
[1] "abcdaebfab" "abcdaebfab"
```

0. paste连接 和 strsplit分割
```R
> u<-paste("abc","de","f")
> u
[1] "abc de f"
> 
> v<-strsplit(u," ")
> v
[[1]]
[1] "abc" "de"  "f" 
```

0. 重复(rep)创建向量
```R
> # repeat
> x<-rep(8,times=4)
> x
[1] 8 8 8 8
> rep(c(5,12,13),3)
[1]  5 12 13  5 12 13  5 12 13
> rep(1:3,2)
[1] 1 2 3 1 2 3
> rep(1:3,each=2)
[1] 1 1 2 2 3 3
```

0. 四舍五入
```R
> y<-c(1.2,3.4,0.4,5.5)
> round(y)
[1] 1 3 0 6
```

0. 等差数列
```R
> seq(from=12,to=20,by=3)
[1] 12 15 18
> seq(from=1.1,to=2,length=10)
 [1] 1.1 1.2 1.3 1.4 1.5 1.6 1.7 1.8 1.9 2.0
> 
> x<-NULL
> for(i in 1:length(x)){
+     print(i)
+ }
[1] 1
[1] 0
> for(i in seq(x)){
+     print(i)
+ }
> # sequence
> seq(x)
integer(0)
```

0. subset(c,f),判断后删除NA
```R
> x<-c(88,NA,12,168,13)
> x[x>50]
[1]  88  NA 168
>
> x<50
[1] FALSE    NA  TRUE FALSE  TRUE
> 
> which(x<50)
[1] 3 5
> 
> 
> # auto delete NA
> subset(x,x>50)
[1]  88 168
> 
> first1<-function(x) return (which(x==1)[1])
> first1(c(3,2,1,0,1,2,3))
[1] 3
```
