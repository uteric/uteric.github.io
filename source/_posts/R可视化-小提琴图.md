---
title: R可视化-小提琴图
date: 2020-02-09 22:49:49
updated: 2020-02-09 22:49:49
tags: R可视化
categories: 编程语言
---

# 一、简单小提琴图 #

小提琴图的作图数据同箱线图，X轴为定性变量，Y轴为定量变量，数据量足够大才有统计意义；

```bash
library(ggplot2)
library(hrbrthemes)

name <- c(rep("A",300),rep("B",300),rep("C",100),rep("C",200),rep("D",300))
value <- c(rnorm(300,10,2),rnorm(300,15,4),rnorm(100,5,1),rnorm(200,12,1),rnorm(300,3,1))
data <- data.frame(name,value)

pviolin <- ggplot(data,aes(x=name,y=value,fill=name))+
geom_violin()+
geom_jitter(color="black", size=0.4, alpha=0.9)+
theme_ipsum()+
theme(legend.position="none")+
ggtitle("violin plot with jitter") +
xlab("")
pviolin
```

# 二、小提琴图优化 #

```bash
# 如果要对X轴进行升序排列，使用reorder(x=x,y=y)函数；
# 若要降序排列，则使用reorder(x=x,y=-y)函数；
pviolin <- ggplot(data,aes(x=reorder(name,-value),y=value,fill=name))+
geom_violin()+
geom_jitter(color="black", size=0.4, alpha=0.9)+
theme_ipsum()+
theme(legend.position="none")+
ggtitle("violin plot with jitter") +
xlab("")
pviolin

# 调节Y轴限度，使用ylim()函数；
ggsave(pviolin,filename="violin4.png",width=12,height=9)
pviolin <- ggplot(data,aes(x=reorder(name,-value),y=value,fill=name))+
geom_violin()+
geom_boxplot(width=0.1,color="grey",alpha=0.3)+
theme_ipsum()+
theme(legend.position="none")+
ggtitle("violin plot with jitter") +
xlab("")+
ylim(0,30)
pviolin

# 绘制水平小提琴图，加上coord_flip()函数；
pviolin <- ggplot(data,aes(x=reorder(name,-value),y=value,fill=name))+
geom_violin()+
geom_jitter(color="black", size=0.4, alpha=0.9)+
theme_ipsum()+
theme(legend.position="none")+
ggtitle("violin plot with jitter") +
xlab("")+
coord_flip()
pviolin

# 同时绘制小提琴图与箱线图；
pviolin <- ggplot(data,aes(x=reorder(name,-value),y=value,fill=name))+
geom_violin()+
geom_boxplot(width=0.1,color="grey",alpha=0.3)+
geom_jitter(color="black", size=0.4, alpha=0.9)+
theme_ipsum()+
theme(legend.position="none")+
ggtitle("violin plot with jitter") +
xlab("")
pviolin
```
