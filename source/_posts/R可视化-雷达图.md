---
title: R可视化-雷达图
date: 2020-02-06 21:12:13
updated: 2020-02-06 21:12:13
tags: R可视化
categories: 编程语言
---

雷达图（radar, spider, or web plot），可以使用R中的fmsb程序包构建；

适合作雷达图的数据是极特别的，只有一行，同时需要对列进行命名；作图之前，需要在数据中添加额外两行，第一行表示最大值，第二行表示最小值；

数据准备就绪后，加载fmsb，并使用radarchart()函数作图即可；

# 一、简单的雷达图 #

```bash
> library(fmsb)
# 作图数据
> var1 <- sample(3:25,15,replace=T)
> var1
[1] 18 20 18 25 23 15 19 12 20 21  3  5 22 21 15
# 通过矩阵平铺成一行；
> data <- matrix(var1,nrow=1)
> data
[,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10] [,11] [,12] [,13] [,14] [,15]
[1,]   18   20   18   25   23   15   19   12   20    21     3     5    22    21    15
# 转化为数据框，以增加数据类型；   
> data <- as.data.frame(data)
> data
V1 V2 V3 V4 V5 V6 V7 V8 V9 V10 V11 V12 V13 V14 V15
1 18 20 18 25 23 15 19 12 20  21   3   5  22  21  15
# 更改列名；
> colnames(data) <- LETTERS[1:15]
> data
A  B  C  D  E  F  G  H  I  J K L  M  N  O
1 18 20 18 25 23 15 19 12 20 21 3 5 22 21 15
# 定义最大值与最小值；
> var2 <- rep(25,15)
> var3 <- rep(0,15)
> var2
[1] 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25
> var3
[1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
# 加入到数据框前两行中，注意顺序；
> data <- rbind(var2,var3,data)
> data
A  B  C  D  E  F  G  H  I  J  K  L  M  N  O
1 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25
2  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
3 18 20 18 25 23 15 19 12 20 21  3  5 22 21 15
# 最后一步，作图；
> p <- radarchart(data)
```

# 二、图形优化 #

主要参数如下：
axistype，坐标轴类型，[0:5]， 0=无标签；1=中心标签；2=周围标签；3=中心与周围标签；4=标签1的分数形式；5=标签3的分数形式；
seg，每个轴的段数，默认为4；
pty，数据点符号，默认16，若指定32，即不加点；
pcol，连接各点的外框线的颜色，默认[1：8]，1=黑色，2=红色，3=绿色，4=蓝色，5=兰色，6=紫色，7=黄色，8=灰色；
plty，连接各点的外框线的类型，默认[1：6]，1=实线，[2:6]=虚线；
plwd，连接各点的外框线的宽度；
pfcol，外框线圈住的面积中的填充色；
cglcol，网格线的颜色；
cglwd，网格线的宽度，默认为1；
cglty，网格线的类型：1=实线；2=虚线；3=细虚线；4=致密虚线，默认为3；
axislabcol，坐标轴标签的颜色；
casixlables，坐标轴刻度，定义最小值，最大值，以及刻度宽度；
title，标题；
centerzero，默认为FALSE，否则从（0，0）为中心作图；
vlcex，组标签字体大小；

```bash
radarchart(data,
axistype=1,seg=5,
pty=16,pcol=rgb(0.2,0.5,0.5,0.9),plty=7,pfcol=rgb(0.2,0.5,0.5,0.5),plwd=3,
cglcol="grey",cglty=1,axislabcol="blue",cglwd=1.5,caxislabels=seq(0,30,5),
vlcex=0.8,title="Radar Plot")
```

# 三、多组叠加 #

注意：尽管多个雷达图可以叠加，但是不要叠加超过3层，否则会造成阅读困难；

```bash
# 数据方面，我们把刚才的数据稍作修改，再添加两行进去即可；
var1 <- sample(3:50,24,replace=T)
data <- matrix(var1,nrow=3)
data <- as.data.frame(data)
colnames(data) <- LETTERS[1:8]
rownames(data) <- paste("sample",letters[1:3],sep="-")
var2 <- rep(50,8)
var3 <- rep(0,8)
data <- rbind(var2,var3,data)
data
radarchart(data)

# 对雷达图进行优化，主要是控制颜色；
color_line=c(rgb(0.2,0.5,0.5,0.9), rgb(0.8,0.2,0.5,0.9) , rgb(0.7,0.5,0.1,0.9))
color_fill=c(rgb(0.2,0.5,0.5,0.4), rgb(0.8,0.2,0.5,0.4) , rgb(0.7,0.5,0.1,0.4))
radarchart(data,
axistype=1,seg=5,
pty=16,pcol=color_line,plty=7,pfcol=color_fill,plwd=3,
cglcol="grey",cglty=1,axislabcol="grey",cglwd=1.5,caxislabels=seq(0,50,10),
vlcex=1.2,title="Multi-Radar Plot")
# 多个图叠加时需要加上图例，否则让人不知所云；
legend(x=0.9,y=1.2,legend=rownames(data[c(3,4,5),]),bty = "n", pch=20 , col=color_line , text.col=color_line, cex=1, pt.cex=1.5)
# 到此为止吧，雷达图尽在于此了。
```