---
title: R语言学习笔记-数据框与矩阵的区别
date: 2020-02-06 20:57:54
updated: 2020-02-06 20:57:54
tags: R语言
categories: 编程语言
---

# 一、数据框(data.frame) #

```bash
> var1=c(1,2,3,4,5,6)
> var2=LETTERS[1:6]
> var3=c(7,8,9,10,11,12)

> data <- data.frame (var1,var2,var3)
> data
var1 var2 var3
1    1    A    7
2    2    B    8
3    3    C    9
4    4    D   10
5    5    E   11
6    6    F   12
# 数据框可以由一系列变量组成，每个变量作为数据框的一列，列名即变量名，变量可以是数字，或字符串，或其他类型；

```

# 二、矩阵(Matrix) #

```bash
> data <- matrix(var1) # 以var1为数据源创建矩阵，结果是6行1列，与matrix(var1,nrow=6,ncol=1相同；
> data
[,1]
[1,]    1
[2,]    2
[3,]    3
[4,]    4
[5,]    5
[6,]    6

> data <- matrix(var1,nrow=1,ncol=6) # 1行6列
> data
[,1] [,2] [,3] [,4] [,5] [,6]
[1,]    1    2    3    4    5    6

> data <- matrix(var1,nrow=2,ncol=3) # 2行3列
> data
[,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> data <- matrix(var1,nrow=3,ncol=2) # 3行2列
> data
[,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6

> data <- matrix(var1,nrow=2,ncol=2) # 若矩阵中的元素少于数据源，则会将剩余的部分舍去；
> data
[,1] [,2]
[1,]    1    3
[2,]    2    4

> data <- matrix(var1,nrow=2,ncol=4) # 若矩阵中的元素多于数据源，则会报错；
Warning message:
In matrix(var1, nrow = 2, ncol = 4) :
data length [6] is not a sub-multiple or multiple of the number of columns [4]

> data <- matrix(var2) # 字符串也可以作为矩阵元素；
> data
[,1]
[1,] "A" 
[2,] "B" 
[3,] "C" 
[4,] "D" 
[5,] "E" 
[6,] "F" 
```