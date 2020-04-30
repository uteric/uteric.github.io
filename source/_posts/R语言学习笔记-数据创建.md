---
title: R语言学习笔记-数据创建
date: 2020-02-06 22:56:21
updated: 2020-02-06 22:56:21
tags: R语言
categories: 编程语言
---

# 一、直接读取文件中的数据 #

```bash
#直接读取.csv文件，本地或网络文件都可以；
data1 <- read.table("https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/1_OneNum.csv", header=TRUE)
head(data1)
price
1    75
2   104
3   369
4   300
5    92
6    64
#对比看一下header=FALSE的结果，列名将被当作第一行，需要特别注意；
data1 <- read.table("https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/1_OneNum.csv", header=FALSE)
head(data2)
V1
1 price
2  75.0
3 104.0
4 369.0
5 300.0
6  92.0
#可以发现，这是一个只有一列的文件，列名为price，9996行；
#此类数据适合作「密度图」
#若要取出price中满足特定条件的数值，可以使用filter()函数，用法如下：
p <- filter(data,price<100)
nrow(p)
#[1] 4799
#也可以直接传递数值，如下：
data %>%
filter( price<300 ) %>%

```

# 二、通过函数创建数据 #

```bash
#创建数据框
#方法一：手动添加数据，仅在数据量小的情况下，谨慎使用；
var1=c("A","B","C","D","E") #对于string，需要用""括起来；
var2=c(3,5,1,10,8) #对于numeric，不要使用"";
data=data.frame (var1,var2)
head(data)
var1 var2
1    A    3
2    B    5
3    C    1
4    D   10
5    E    8
#也可以使用如下方法创建数据框，效果是相同的；
data <- data.frame (
var1=c("A","B","C","D","E"),
var2=c(3,5,1,10,8)
)
#此类数据有两列，一列为分类变量（categorical variable），一列为数值变量（numeric variable），适合作条形图，如；
ggplot(data,aes(x=var1,y=var2))+geom_bar(stat="identity") #stat="identity表明直接引用数据集里变量的值作为纵坐标的高度；

#方法二：使用rnorm()函数，rnorm(n, mean = 0, sd = 1)，n为产生随机值个数，mean是平均数，sd是标准差；默认平均数为0，标准差为1，呈正态分布；
var1=rnorm(100)
var2=rnorm(100,mean=2,sd=0.5)
head(var1)
[1]  0.7376882  0.9055453 -0.1456011  0.8596800 -1.5881099  1.4858029 #var1在0左右分布；
head(var2)
[1] 3.309464 2.371139 1.848107 2.126949 2.006115 2.082000 #var2在2左右分布；
#创建数据框
data <- data.frame (var1,var2)
head(data)
var1     var2
1  0.7376882 3.309464
2  0.9055453 2.371139
3 -0.1456011 1.848107
4  0.8596800 2.126949
5 -1.5881099 2.006115
6  1.4858029 2.082000
#此类数据有两列，两列都是数值，适合作散点图；

#方法三：使用sample(seq())函数创建数值变量；
data <- data.frame (
var1=sample(seq(1,9),20,replace=T), #replace=T，允许数值重复；
var2=sample(seq(1,30),20,replace=F) #replace=F，数值不能重复；
)
head(data)
var1 var2
1    3   16
2    2    5
3    3   28
4    6   30
5    2   10
6    3   17

#方法四：使用sample()函数创建数值变量；
data <- data.frame (
var1=sample(3:20,30,replace=T), #3－20之间取30个数，允许重复；
var2=sample(21:30,30,replace=T) #21－30之间取30个数，允许重复；
)
head(data)
var1 var2
1   15   21
2   14   21
3   17   27
4   10   29
5    7   27
6   12   24

#方法五：使用LETTERS创建字母；
var1=letters[1:7] #letters取小写字母；
var2=LETTERS[8:14] #LETTERS取大写字母；
var1
[1] "a" "b" "c" "d" "e" "f" "g"
var2
[1] "H" "I" "J" "K" "L" "M" "N"

#方法六：使用abs()函数取绝对值；
var1=abs(rnorm(10,mean=0,sd=3))
var1
2.8804684 1.8650338 0.3870626 1.2668471 5.1131199 4.6320510 2.8822397 0.2476684 7.1651732 3.2236713

#方法七：使用seq()函数创建数值变量；
var1=seq(1,9,length.out=10) # 从1至9，产生10个数，两两之间是等距的；
var2=seq(1,9,2) # 从1至9，以2为间隔产生数列；
var1
1.000000 1.888889 2.777778 3.666667 4.555556 5.444444 6.333333 7.222222 8.111111 9.000000
var2
1 3 5 7 9

#方法八，使用paste()创建分类变量；
var1=paste("sample",seq(1,10),sep="") # 结合字符串与数字，中间没有分隔；
var2=paste("sample",seq(1,10),sep="-") # 结合字符串与数字，中间以－分隔；
var1
"sample1"  "sample2"  "sample3"  "sample4"  "sample5"  "sample6" "sample7"  "sample8"  "sample9"  "sample10"
var2
"sample-1"  "sample-2"  "sample-3"  "sample-4"  "sample-5" "sample-6"  "sample-7"  "sample-8"  "sample-9"  "sample-10"

#方法九，使用mutate()函数向数据框末端追加一列；
data <- data.frame (
var1=rnorm(200,mean=1,sd=1),
var2=rnorm(200,mean=3,sd=0.3)
)
head(data)
var1     var2
1  1.39138057 2.816622
2  1.19333387 3.243189
3 -0.07646219 2.950697
4 -0.39530844 2.954534
5  0.52766461 2.923421
6  0.34335782 2.870595
#下面使用mutate()函数追加新的一列var3;
data <-mutate(data,var3=paste("sample",seq(1,200),sep="")) #mutate()本身并不改变data的值，只会生成一个新的数据框，因此要重新对data赋值；
> head(data)
var1     var2    var3
1  1.39138057 2.816622 sample1
2  1.19333387 3.243189 sample2
3 -0.07646219 2.950697 sample3
4 -0.39530844 2.954534 sample4
5  0.52766461 2.923421 sample5
6  0.34335782 2.870595 sample6

# mutate()也可以结合ifelse()函数追加分类；
var1=sample(1:30,10,replace=T)
var2=sample(31:45,10,replace=T)
data <- data.frame (var1,var2)
var3=ifelse(var1>15,"type1","type2")
data <- mutate(data,var3) # 或者可以写成，
data
var1 var2  var3
1     1   35 type2
2    10   37 type2
3     1   44 type2
4    26   37 type1
5     1   35 type2
6    16   42 type1
7    12   45 type2
8     5   31 type2
9    26   41 type1
10    8   43 type2

#方法十：使用rep()函数添加数值或分类变量；
var1=rep(1:4,3)
var2=rep(1:4,each=3)
var3=rep(1:4,each=3,len=10)
var1
1 2 3 4 1 2 3 4 1 2 3 4
var2
1 1 1 2 2 2 3 3 3 4 4 4
var3
1 1 1 2 2 2 3 3 3 4
#同理，可以结合mutate()和rep()来追加分类变量；
var1=sample(2:50,10,replace=T)
var2=sample(40:60,10,replace=T)
data <- data.frame (var1,var2)
var3=rep(c("class1","class2"),each=5)
data <- mutate(data,var3)
data
var1 var2   var3
1    49   58 class1
2    39   47 class1
3    18   47 class1
4    17   54 class1
5    25   52 class1
6    22   43 class2
7     4   59 class2
8    23   45 class2
9    41   47 class2
10   43   40 class2

```
