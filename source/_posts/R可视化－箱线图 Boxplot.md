---
title: R可视化－箱线图 Boxplot
date: 2020-02-07 21:09:39
updated: 2020-02-07 21:09:39
tags: R可视化
categories: 编程语言
---


```bash
library(tidyverse)
# tidyverse，包含ggplot2；
library(hrbrthemes)
# hrbrthemes，主题包；
library(viridis)
# Viridis，，配色包，是从Python移植到R的，共有四种配色：A=magma，B=plasma，C=inferno，D=viridis，默认为D，语法如下：
# scale_fill_viridis(..., alpha = 1, begin = 0, end = 1, direction = 1, discrete = FALSE, option = "D")
# 对于离散型的数据，需要指定discrete=TRUE，否则会报错；alpha，指定透明度； 

name <- c(rep("A",300),rep("B",300),rep("C",100),rep("C",200),rep("D",300))
value <- c(rnorm(300,10,2),rnorm(300,15,4),rnorm(100,5,1),rnorm(200,12,1),rnorm(300,3,1))
data <- data.frame(name,value)
pbox <- ggplot(data,aes(x=name,y=value,fill=name))+
geom_boxplot()+
scale_fill_viridis(option="D",discrete = TRUE, alpha=0.6)+
geom_jitter(color="black", size=0.4, alpha=0.9)+ # geom_jitter()，ggplot2的程序包，绘制随机抖动散点；
theme_ipsum()+
theme(legend.position="none")+ # 去掉图例；
ggtitle("A boxplot with jitter") +
xlab("")
pbox
ggsave(pbox,filename="box.png",width=12,height=9)
```

关于箱线图的数据类型：
X轴为定性变量，如本例中的A，B，C，D；Y轴为定量变量，即具体的数值；箱线图可以用来观察数据的分散情况，以及是否有异常值；因此，本例中使用rnorm()函数产生的正态分布数据作为输入；而对于本例中的分类C，明显由两组数据组成，绘制小提琴图或许是更直观的方式；

补充说明：hrbrthemes，是ggplot2的附加主题，如常用的theme_ipsum()，可以对图形重新排版，对标题，副标题等的字体，字号，加粗，坐标轴，网格线，刻度线等都进行了定义，另外，可以调节坐标轴标签的位置，参数为[blmcrt]，默认为[rt]；遗憾的是，并不能通过这个程序添加标题，副标题，或者坐标轴标签，因此，需要结合theme(), ggtitle()与xlab(),ylab()；语法如下：

```bash
theme_ipsum(base_family = "Arial Narrow", base_size = 11.5,
plot_title_family = base_family, plot_title_size = 18,
plot_title_face = "bold", plot_title_margin = 10,
subtitle_family = base_family, subtitle_size = 12,
subtitle_face = "plain", subtitle_margin = 15,
strip_text_family = base_family, strip_text_size = 12,
strip_text_face = "plain", caption_family = base_family,
caption_size = 9, caption_face = "italic", caption_margin = 10,
axis_text_size = base_size, axis_title_family = subtitle_family,
axis_title_size = 9, axis_title_face = "plain",
axis_title_just = "rt", plot_margin = margin(30, 30, 30, 30),
grid_col = "#cccccc", grid = TRUE, axis_col = "#cccccc",
axis = FALSE, ticks = FALSE)
```