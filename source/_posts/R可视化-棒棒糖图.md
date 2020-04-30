---
title: R可视化-棒棒糖图
date: 2020-02-11 22:51:40
updated: 2020-02-11 22:51:40
tags: R可视化
categories: 编程语言
---


棒棒糖图，介于柱形图与散点图之间，之所以这么说，是因为它能以柱形图与散点图的数据作图，即两列定量变量（数字），或一列定量变量（数字）一列分类变量（字符串）；

有趣的是，棒棒糖图其实不是由一个函数绘制的，而是由两个函数叠加成的，即使用geom_point()绘制点，使用geom_segment()绘制线；

```bash
library(ggplot2)
var1 <- LETTERS[1:20]
var2 <- runif(20,min=2,max=20)
var3 <- runif(20,min=1,max=5)
var4 <- c(rep("classI",10),rep("classII",10))
data <- data.frame(var1,var2,var3,var4)
> data
var1      var2     var3    var4
1     A  6.266182 3.434602  classI
2     B 12.758686 3.288259  classI
3     C 10.340497 1.924543  classI
4     D  6.177109 2.720639  classI
5     E 16.549641 2.546006  classI
6     F  2.460816 1.659354  classI
7     G  9.812373 2.556147  classI
8     H  3.367085 3.515422  classI
9     I  6.891384 1.811289  classI
10    J  8.513522 2.264906  classI
11    K 16.506077 1.968184 classII
12    L  2.638983 4.739859 classII
13    M 19.169797 3.854552 classII
14    N 12.261985 2.441648 classII
15    O 16.296435 4.560705 classII
16    P 13.804297 2.391673 classII
17    Q  2.207188 1.330717 classII
18    R 13.063605 3.532234 classII
19    S 17.489978 4.359878 classII
20    T 15.389995 4.394447 classII
# 数据准备好了，现在开始正式作图；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point()+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2))
plolli
ggsave(plolli,filename="lollipop1.png",width=12,height=9)
```

# 着重解释一下geom_segment()函数的用法，如下：

```bash
geom_segment(x,y,xend,yend,color,size,linetype,alpha)
# 其中，x=xstart,y=ystart,xend=xend,yend=yend，表示x与y的起始位置与终止位置；
```

```bash
# 优化散点的大小，形状，颜色，填充，透明度；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black",
fill="69b3a2",
size=3,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2))
plolli
# 优化线条的大小，线型，颜色，透明度；
# 其中，linetype数值为[0:6]，0 = blank, 1 = solid, 2 = dashed, 3 = dotted, 4 = dotdash, 5 = longdash, 6 = twodash；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black",
fill="69b3a2",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2),
color="black",
alpha=0.4,
size=1.2,
linetype=3)
plolli
# 优化主题；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black",
fill="69b3a2",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2),
color="black",
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank())
plolli
# 转换为水平版本；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black",
fill="69b3a2",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2),
color="black",
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank())+
xlab("")+
ylab("")+
coord_flip()
plolli
# 改变基线位置，默认为0；
plolli <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black",
fill="69b3a2",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=10,yend=var2),
color="black",
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank())+
xlab("")+
ylab("")
plolli
# 对数据分组，并对散点填充不同颜色；
plolli <- ggplot(data,aes(x=var1,y=var2,fill=var4))+
geom_point(color="black",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=10,yend=var2),
color="black",
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank(),
legend.position = "none")+
xlab("")+
ylab("")
plolli
#分组，并对线填充不同颜色；
plolli <- ggplot(data,aes(x=var1,y=var2,fill=var4))+
geom_point(color="black",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=10,yend=var2,color=var4),
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank(),
legend.position = "none")+
xlab("")+
ylab("")
plolli
# 排序；
data <- data.frame(var1,var2,var3,var4)
data <- arrange(data,var2)
data[,1]=factor(data[,1],levels=data[,1],order=T)
plolli <- ggplot(data,aes(x=var1,y=var2,fill=var4))+
geom_point(color="black",
size=4,
shape=22,
alpha=0.8)+
geom_segment(aes(x=var1,xend=var1,y=0,yend=var2,color=var4),
alpha=0.8,
size=1,
linetype=1)+
theme_ipsum()+
theme(panel.grid = element_blank(),
legend.position = "none")+
xlab("")+
ylab("")
plolli
# 克利夫兰点图，比较每个实体中的两个点的关系；
plolli <- ggplot(data,aes(x=var1,y=var2,fill=var4))+
geom_segment(aes(x=var1,xend=var1,y=var2,yend=var3),linetype=2)+
geom_point(aes(x=var1,y=var2),color=alpha("red",0.5),size=3)+
geom_point(aes(x=var1,y=var3),color=alpha("blue",0.5),size=3)+
coord_flip()+
theme_ipsum()+
theme(legend.position = "none",
panel.grid.major.y = element_blank())+
xlab("")+
ylab("")
plolli
# 克利夫兰点图，若不进行排序，会显得图片杂乱无章，因此，最好先排序后作图，下面这个例子是以平均值从小到大作图；
library(ggplot2)
library(hrbrthemes)
library(dplyr)
var1 <- LETTERS[1:20]
var2 <- runif(20,min=2,max=20)
var3 <- runif(20,min=1,max=5)
var4 <- c(rep("classI",10),rep("classII",10))
data <- data.frame(var1,var2,var3,var4)
data <- mutate(data,var5=(var2+var3)/2) #加入新的一列var5，其值为var2与var3的平均值；
data <- arrange(data,var5) # 以
data[,1]=factor(data[,1],levels=data[,1],order=T)
plolli <- ggplot(data,aes(x=var1,y=var2,fill=var4))+
geom_segment(aes(x=var1,xend=var1,y=var2,yend=var3),linetype=2)+
geom_point(aes(x=var1,y=var2),color=alpha("red",0.5),size=3)+
geom_point(aes(x=var1,y=var3),color=alpha("blue",0.5),size=3)+
coord_flip()+
theme_ipsum()+
theme(legend.position = "none",
panel.grid.major.y = element_blank())+
xlab("")+
ylab("")
plolli
```

