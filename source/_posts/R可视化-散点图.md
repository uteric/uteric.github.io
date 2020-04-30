---
title: R可视化-散点图
date: 2020-02-11 22:51:29
updated: 2020-02-11 22:51:29
tags: R可视化
categories: 编程语言
---


在绘图之前，先谈谈散点图需要的数据类型。散点，顾名思义，就是很多点分散在平面上，而在平面坐标中，确定一个点需要两个坐标值，因此，绘制散点图的数据需要两列，且两列都是数字，对应的每一行都是平面坐标中的一个点。

另外，样本大小没有限制，少到一个点，多到成千上万，都是可以的；对于有规律的散点，甚至也可以拟合平滑曲线呈现其趋势。

# 一、绘制基本的散点图 #

```bash
# 生成随机数据；
var1 <- runif(200) # runif用来生成介于min与max之间的随机数，默认0至1之间；
var2 <- runif(200,min=3,max=15)
data <- data.frame(var1,var2) # 通过runif()生成的数据是毫无规律可言的，是名副其实的散点；

# 绘制散点图；
pscatter <- ggplot(data,aes(x=var1,y=var2))+
geom_point() # 绘制散点图的函数；
pscatter
ggsave(pscatter,filename="scatter.png",width=12,height=9)
```

# 二、散点图优化 #

```bash
# 对散点进行修饰，改变散点大小，形状，颜色，透明度，与笔触；
pscatter <- ggplot(data,aes(x=var1,y=var2))+
geom_point(color="black", # 笔触颜色；
fill="#69b3a2", #填充色；
size=4,  # 大小；
shape=22, # 形状；
alpha=0.8, # 透明度；
stroke=1)+ # 笔触大小；
theme_ipsum()
pscatter

# 对散点进行分类；
# 为方便分类，在此我们给data添加新的一列var3，并以var3分组进行填充；
var1 <- runif(200)
var2 <- runif(200,min=3,max=15)
var3 <- c(rep("A",30),rep("B",60),rep("C",110))
data <- data.frame(var1,var2,var3)
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(size=4,
shape=24,
alpha=0.8,
stroke=1)+
theme_ipsum()
pscatter

# 以var3分组设定形状；
pscatter <- ggplot(data,aes(x=var1,y=var2,shape=var3))+
geom_point(size=4,
color="#69b3a2",
alpha=0.9,
stroke=1)+
theme_ipsum()
pscatter

# 以var3分组设定大小；
pscatter <- ggplot(data,aes(x=var1,y=var2,size=var3))+
geom_point(color="black",
fill="#69b3a2",
alpha=0.9,
shape=22,
stroke=1)+
theme_ipsum()
pscatter

# 以var3分组设定透明度；
pscatter <- ggplot(data,aes(x=var1,y=var2,alpha=var3))+
geom_point(color="black",
fill="#69b3a2",
size=4,
shape=22,
stroke=1)+
theme_ipsum()
pscatter

# 以var3分组，同时设定形状，透明度，大小，颜色；
pscatter <- ggplot(data,aes(x=var1,y=var2,shape=var3,alpha=var3,size=var3,color=var3))+
geom_point()+
theme_ipsum()
pscatter

```

# 三、给散点添加文字标签 #

```bash
# 使用geom_text()函数添加文字；
var1 <- runif(26)
var2 <- runif(26,min=3,max=15)
var3 <- c(rep("classI",6),rep("classII",9),rep("classIII",11))
var4 <- LETTERS[1:26]
data <- data.frame(var1,var2,var3,var4)
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=4,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
geom_text(label=var4, # 指定标签来源
nudge_x=0.02,nudge_y=0.02, # 标签位置的偏移量；
check_overlap = T) # 尽量不要重叠；
pscatter

# 使用geom_label()函数添加文字标签，效果与geom_text()不同，会显示外框；
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=4,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
geom_label(label=var4,
nudge_x=0.02,nudge_y=0.02,
check_overlap = T,
fill="white",
color="blue")
pscatter

# 只添加一个文字标签；
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=4,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
geom_label(label="important", # 直接以字符串形式指定标签内容；
x=0.5, # 位置
y=6.5,
fill="white",
color="blue")
pscatter

# 仅对选择的点添加文字标签；
library(dplyr) # 调用dplyr包中的filter()函数进行数据筛选；
var1 <- runif(26)
var2 <- runif(26,min=3,max=15)
var3 <- c(rep("classI",6),rep("classII",9),rep("classIII",11))
var4 <- LETTERS[1:26]
data <- data.frame(var1,var2,var3,var4)
data2 <- filter(data,var1>0.5 & var2>9)
data2
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=4,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
geom_label(data=data2,
aes(label=var4), # 语法格式为(data=,aes(label=));
nudge_x=0.03,nudge_y=0.03,
check_overlap = T,
fill="transparent")
pscatter
```

# 为坐标轴添加触须 #

```bash
# 使用geom_rug()添加触须，并指定颜色，透明度，和大小；
var1 <- runif(26)
var2 <- runif(26,min=3,max=15)
var3 <- c(rep("classI",6),rep("classII",9),rep("classIII",11))
var4 <- LETTERS[1:26]
data <- data.frame(var1,var2,var3,var4)
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=4,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
geom_rug(col="steelblue",alpha=0.7, size=0.9)
pscatter
```

# 四、添加边际分布（Marginal distribution） #

边际分布可以通过ggExtra:ggMarginal()函数添加，用法如下：

```bash
ggMarginal(p, data, x, y, type = c("density", "histogram", "boxplot",
"violin", "densigram"), margins = c("both", "x", "y"), size = 5, ...,
xparams = list(), yparams = list(), groupColour = FALSE,
groupFill = FALSE,color,fill)
# type必须是五种中的一种；margins决定呈现在X轴，Y轴或双侧（默认）；size决定大小；
```

```bash
library(dplyr)
library(ggplot2)
library(hrbrthemes)
library(ggExtra)
var1 <- runif(26)
var2 <- runif(26,min=3,max=15)
var3 <- c(rep("classI",6),rep("classII",9),rep("classIII",11))
var4 <- LETTERS[1:26]
data <- data.frame(var1,var2,var3,var4)
pscatter <- ggplot(data,aes(x=var1,y=var2,fill=var3))+
geom_point(color="black",
size=3,
alpha=0.8,
shape=22,
stroke=1)+
theme_ipsum()+
theme(legend.position = "none")+
geom_rug(col="steelblue",alpha=0.7, size=0.9)
pscatter1 <- ggMarginal(pscatter,type="densigram")
pscatter1

# 进一步优化，指定大小（数字越大边际占比越小），添加填充色；
```

# 五、添加平滑趋势线 #

# 使用函数geom_smooth()添加趋势线，用法如下：

```bash
geom_smooth(mapping = NULL, data = NULL, stat = "smooth",
position = "identity", ..., method = "auto", formula = y ~ x,
se = TRUE, na.rm = FALSE, show.legend = NA, inherit.aes = TRUE)
```

```bash
var5 <- 1:200+rnorm(200,sd=2)
var6 <- 1:200+rnorm(200,sd=5)
var7 <- rep(c("classI","classII"),each=100)
data <- data.frame(var5,var6,var7)
head(data)

pscatter <- ggplot(data,aes(x=var5,y=var6,fill=var7))+
geom_point(color="blue",
size=0.9)+
geom_smooth(method=lm,color="red",se=F)
pscatter

```

