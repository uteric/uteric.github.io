---
title: R可视化-线图
date: 2020-02-11 22:51:34
updated: 2020-02-11 22:51:34
tags: R可视化
categories: 编程语言
---


# 一、绘制基本线图 #

对于线图的作图数据，至少两列，且两列都是数值变量；从本质上讲，其实就是散点图加上连线；

```bash
library(ggplot2)
library(hrbrthemes)
# 作图数据；
var1<- seq(1:100)
var2<- seq(1:100)+rnorm(100,mean=1,sd=2)
data <- data.frame(var1,var2)
# 线图；
pline <- ggplot(data,aes(x=var1,y=var2))+
geom_line(alpha=0.9,size=1,linetype=6,color="#69b3a2")+
theme_ipsum()+
xlab("")+
ylab("")+
pline
ggsave(pline,filename="line01.png",width=12,height=9)

```
# 二、对数转换 #

有时，数据在坐标轴上跨度很大，如果取大单位，则小数据会看不到，这时候可以对坐标轴作对数转换；
注意：实际作图时坐标轴的值为真值，并不是取对数后的值，但是坐标轴的刻度却是以对数值分布的，比如（10，100，1000，100）分别对应坐标轴上的（1，2，3，4）的位置，但显示的刻度却是（10，100，1000，10000）；
对数转换有个极大的优点，即放大了曲线的下端；
本例中仅对Y轴作对数转换，也可以对x轴进行对数转换，或根据需要同时转换；

```bash
pline <- ggplot(data,aes(x=var1,y=var2))+
geom_line(alpha=0.9,size=1,linetype=6,color="#69b3a2")+
theme_ipsum()+
xlab("")+
ylab("")+
scale_y_log10()
pline
```
# 三、显示数据点 # 

```bash
pline <- ggplot(data,aes(x=var1,y=var2))+
geom_line(alpha=0.9,size=1,linetype=1,color="steelblue")+
theme_ipsum()+
xlab("")+
ylab("")+
geom_point(alpha=0.9,size=1.5,shape=21,fill="blue")
pline
```

# 四、分组 #

```bash
var1 <- c(rep("A",30),rep("B",30),rep("C",30))
var2 <- c(rnorm(30),rnorm(30,mean=3),rnorm(30,mean=9))
var3 <- c(rnorm(30,mean=4),rnorm(30,mean=2),rnorm(30,mean=1))
data <- data.frame(var1,var2,var3)
pline <- ggplot(data,aes(x=var2,y=var3,group=var1,color=var1))+
geom_line()+
geom_point()+
theme_ipsum()+
xlab("")+
ylab("")
pline
```

# 五、添加水平与垂直标度线，注释点，注释文字 #
```bash
pline <- ggplot(data,aes(x=var2,y=var3,group=var1,color=var1))+
geom_line()+
geom_point()+
theme_ipsum()+
xlab("")+
ylab("")+
geom_hline(yintercept=2.18,color="red",size=0.7,alpha=0.9)+
geom_vline(xintercept=4.47,color="blue",size=0.7,alpha=0.9)+
annotate(geom="point",x=4.47,y=2.18,size=3,fill="transparent",color="red")+
annotate(geom="text",x=5,y=1.5,label="very important",color="red")
pline
```

# 六、使用patchwork绘制双Y轴 #

```bash
library(babynames)
library(dplyr)
library(tidyr)
library(ggrepel)
library(patchwork)

var1 <- c(rep("A",30),rep("B",30),rep("C",30))
var2 <- c(rnorm(30),rnorm(30,mean=3),rnorm(30,mean=9))
var3 <- c(rnorm(30,mean=4),rnorm(30,mean=2),rnorm(30,mean=1))
var4 <- c(1:90)
data <- data.frame(var1,var2,var3,var4)
p1 <- ggplot(data,aes(x=var4,y=var2))+
geom_line(color="blue",size=1)
p1  
p2 <- ggplot(data,aes(x=var4,y=var3))+
geom_line(color="red",size=1)
p2
pline <- p1+p2
pline
```
