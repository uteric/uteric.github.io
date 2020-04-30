---
title: RNASEQ分析入门笔记9-使用pheatmap绘制热图
date: 2018-09-19 00:14:41
updated: 2018-09-19 00:14:41
tags: RNASEQ
categories: 编程语言
---

## 一、安装pheatmap ##

```bash
source("http://bioconductor.org/biocLite.R") #调入安装工具
biocLite("pheatmap") # 安装pheatmap
library(pheatmap) # 检测是否安装成功
```
## 二、读取数据 ##

```bash
setwd("E:/R") # 设置工作路径，本例是在E盘下的R文件夹，如果要查询当前工作路径，则使用getwd()命令，其中，wd=working directory
data <- read.delim("clipboard", header = T, sep = "\t", row.names = 1) # read.delim意思是读取delimited text files，即分隔符分隔的文件
```
read.delim这个命令有必要详细解释一下，其格式为：read.delim(file, header=TRUE, sep="\t")

其中，第一个参数是文件的位置，本例中直接使用剪贴板的数据，也可以写文件名；header=T，表示第一行是列名；sep = "\t"指定分隔符类型为制表符，sep = ","表示分隔符为逗号，sep = " "表示制表符为空格；row.names=1，表示第一列包含行名。

## 三、绘制热图 ##

```bash
pheatmap(data,scale="row",color=colorRampPalette(c("navy","white","firebrick3"))(50),fontsize_row=3,fontsize_col=10)
```

scale # 对矩阵进行标准化，这一步是必要的，尤其是数据之间差异较大时效果非常明显，scale="row"，对行进行标准化；scale="column“，对列进行标准化；默认为none；
color # 指定小格的颜色；
fontsize_row，指定行字号；fontsize_col，指定列字号；

## 四、参数设置 ##

```bash
display_numbers=TRUE # 在热图格子中展示文本
number_format = "%.2f" # 热图中文本保留小数点后两位
number_format = "%.1e" # 热图中文本使用科学计数法
number_color="purple" # 热图中文本的颜色
cluster_cols=F # 不要对列进行聚类，默认是T
cluster_rows=F # 不要对行进行聚类，默认是T 
pheatmap(data,scale="row",cellwidth=15,cellheight=12,main="example heatmap",fontsize=8,filename="test.pdf")
# cellwidth，设置格子宽度；cellheight，设置格子高度
# main，设置标题；fontsize，标题字体
# filename，直接指定保存为文件，支持格式png, pdf, tiff, bmp, jpeg
```