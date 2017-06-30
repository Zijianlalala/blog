---
title: R数据框
date: 2017-05-24 09:13:10
tags: R语言
category: R语言
---

0. 数据框data.frame,数据一般从文件或者数据库中读取
```R
> d<-data.frame(list(kid=c("Jack","Jill"),age=c(12,10)))
> d
   kid age
1 Jack  12
2 Jill  10
```

0. 从文件中读取
```R
> example <- read.table("test",header=TRUE)

```

0. 访问数据框
```R
> d$kid
[1] Jack Jill
Levels: Jack Jill
> d[1]
   kid
1 Jack
2 Jill
> d[[1]]
[1] Jack Jill
Levels: Jack Jill
> d[,1]
[1] Jack Jill
Levels: Jack Jill
> d[1,]
   kid age
1 Jack  12
```

0. 合并merge(x,y) 删除不匹配的 merge(x,y,by.x="列名",by.y="列名") 列名不同但意义相同
```R  
> d1
     kids states
1    Jack     CA
2    Jill     MA
3 Jillian     MA
4    john     HI
> d2
     kids ages
1    Jack   10
2    Jill    7
3 Lillian   12
> merge(d1,d2)
  kids states ages
1 Jack     CA   10
2 Jill     MA    7
```

0. 统计文件中没一行的属性个数
```R
count.fields(file, sep = "", quote = "\"'", skip = 0,
             blank.lines.skip = TRUE, comment.char = "#")
```

