---
title: R基本语法
date: 2017-05-23 21:22:02
tags: R语言
---
* 求模运算
```R
> 1%%2
[1] 1
> 2%%2
[1] 0
> 3%%2
[1] 1
```

* 求商,取整
```R
> 3%/%2
[1] 1
```

* all 和 any
```R
> x<-1:10
> y<-x>8
> all(y)
[1] FALSE
> any(y)
[1] TRUE
```

* 判断两个向量相等,建议使用ALL(a==b).identical对于不同的类型相同的值判断为不同
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

* 设置名字
```R
> x<-1:2
> names(x)<-c("a","b")
> x
a b 
1 2 
```

* 长度和样式(数据类型)
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

* ifelse语句,类似c++中的条件表达式?:
```R
> x<-1:10
> y<-ifelse(x%%2,5,12)
> y
 [1]  5 12  5 12  5 12  5 12  5 12
```

* 带默认值的函数,布尔值建议用TRUE/FALSE,而不是T/F
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

* 向量(vector)按行结合(rbind),按列结合(cbind)
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

* 均值和标准差
```R
> x<-c(1,2,3)
> x[2:3]
[1] 2 3
> mean(x)
[1] 2
> sd(x)
[1] 1
```

* 列表,赋值,读取值的三种方式
```R
> x<-list(u=2,v="abc")
> x
$u
[1] 2

$v
[1] "abc"

> x$u
[1] 2
> 
> # str的意思是structure,而不是string
> str(x)
List of 2
 $ u: num 2
 $ v: chr "abc"
> 
> j<-list(name="Joe",salary=55000,union=TRUE)
> j$name
[1] "Joe"
> j[[1]]
[1] "Joe"
> j[1]
$name
[1] "Joe"

> j["name"]
$name
[1] "Joe"

> 
> z<-vector(mode="list")
> z
list()
> z[["abc"]]<-3
```

* sapply 和 函数
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

* 数据框,数据一般从文件或者数据库中读取
```R
> d<-data.frame(list(kid=c("Jack","Jill"),age=c(12,10)))
> d
   kid age
1 Jack  12
2 Jill  10
```

* 矩阵中取出一维的矩阵,自动降为向量,为了防止自动降维,使用参数drop=FALSE
```R
> z<-matrix(1:8,ncol=2)
> r<-z[2,,drop=FALSE]
> r
     [,1] [,2]
[1,]    2    6
```

* apply 函数应用
```R
> # apply(m,dimcode,f,fargs)
> z<-matrix(1:6,ncol=2)
> z
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
> apply(z,2,mean)
[1] 2 5
> matrix(apply(z,1,mean))
     [,1]
[1,]  2.5
[2,]  3.5
[3,]  4.5
> 
> x<-matrix(1:6,ncol=2)
> y<-matrix(1:6,ncol=3)
> z<-matrix(1:6,ncol=2,byrow = TRUE)
> x
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
> y
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
> z
     [,1] [,2]
[1,]    1    2
[2,]    3    4
[3,]    5    6
> 
> 3 * x
     [,1] [,2]
[1,]    3   12
[2,]    6   15
[3,]    9   18
> x%*%y
     [,1] [,2] [,3]
[1,]    9   19   29
[2,]   12   26   40
[3,]   15   33   51
> 
> a<-matrix(1:12,ncol=3)
> a
     [,1] [,2] [,3]
[1,]    1    5    9
[2,]    2    6   10
[3,]    3    7   11
[4,]    4    8   12
> a[,2]
[1] 5 6 7 8
> a[,2:3]
     [,1] [,2]
[1,]    5    9
[2,]    6   10
[3,]    7   11
[4,]    8   12
> a[1:2,1:2]=matrix(rep(0,times=4))
> a
     [,1] [,2] [,3]
[1,]    0    0    9
[2,]    0    0   10
[3,]    3    7   11
[4,]    4    8   12
> a[,-2]
     [,1] [,2]
[1,]    0    9
[2,]    0   10
[3,]    3   11
[4,]    4   12
```

* NA和NULL
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

* nchar,toupper,tolower
```R
> x<-"abcdaebfab"
> nchar(x)
[1] 10
> y<-toupper(x)
> y
[1] "ABCDAEBFAB"
> 
> chartr("ab","xy",x)
[1] "xycdxeyfxy"
> sub("ab","xy",x)
[1] "xycdaebfab"
> gsub("ab","xy",x)
[1] "xycdaebfxy"
> 
> rep(x,times=3,each=2,length.out=2)
[1] "abcdaebfab" "abcdaebfab"
```

* paste连接 和 strsplit分割
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

* 新建PDF文件,写入直方图,关闭文件,写入硬盘,rnorm(100),正态分布,100个变量
```R
pdf("xh.pdf")
hist(rnorm(100))
dev.off()
```

* 重复创建向量
```R
> # reqeat
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

* 四舍五入
```R
> y<-c(1.2,3.4,0.4,5.5)
> round(y)
[1] 1 3 0 6
```

* 等差数列
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

* subset(向量,条件),判断后删除NA
```R
> x<-c(88,NA,12,168,13)
> x
[1]  88  NA  12 168  13
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

* 直方图和summary
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










