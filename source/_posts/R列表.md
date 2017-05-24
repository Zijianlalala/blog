---
title: R列表
date: 2017-05-24 09:12:19
tags: R语言
---

0. 赋值,索引
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
> z
$abc
[1] 3
```

0. 添加组件
```R
> x<-list(u=2,v="abc")
> x$w<-"hahahah"
> x
$u
[1] 2

$v
[1] "abc"

$w
[1] "hahahah"

> x[[4]<-"asd"
> x[5:7]<-5:7
> x
$u
[1] 2

$v
[1] "abc"

$w
[1] "hahahah"

[[4]]
[1] "asd"

[[5]]
[1] 5

[[6]]
[1] 6

[[7]]
[1] 7
```

0. 删除组件,赋值NULL即可
```R
>   x[[4]]<-NULL
> x
$u
[1] 2

$v
[1] "abc"

$w
[1] "hahahah"

[[4]]
[1] 5

[[5]]
[1] 6

[[6]]
[1] 7
```

0. 获取长度
```R
> length(x)
[1] 6
```
