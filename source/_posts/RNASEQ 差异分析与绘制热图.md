---
title: RNASEQ分析入门笔记10-差异分析与绘制热图（命令集合）
date: 2018-10-05 23:19:13
updated: 2018-10-05 23:19:13
tags: RNASEQ
categories: 编程语言
---

## 一、差异分析 ##

```bash
setwd("E:/R/My Analysis/") # 设置工作目录
getwd() # 查看当前工作目录
dir() # 查看工作目录下的文件

library(DESeq2) # 载入DESeq2

data <- read.delim("clipboard",header=T,row.names=1) # 从剪贴板载入要分析的数据

countData <- data
condition <- factor(c("S","S","R","R")) # 定义condition，根据具体情况作相应改变
colData <- data.frame(row.names=colnames(countData),condition)
condition # 查看condition
colData # 查看colData是否与预期一致

dds <- DESeqDataSetFromMatrix(countData,DataFrame(condition),design= ~ condition) # 构建dds矩阵
head(dds) # 查看dds矩阵的前端
dds2 <- DESeq(dds) # 对dds矩阵进行Normalize

res <- results(dds2) # 使用results()函数获取结果，并赋值给res
summary(res) # 查看res矩阵的总结信息
table(res$padj<0.05) # 取padj小于0.05的数据，作为第一个checkpoint
res <- res[order(res$padj),] # 按照padj的大小将res重新排列

diff <- subset(res,padj < 0.05 & (log2FoldChange >1 | log2FoldChange < -1)) # 获取padj小于0.05，表达倍数取以2为对数后绝对值大于1的差异表达基因
nrow(diff) # 查看行数，作为第二个checkpoint

resdata <- merge(as.data.frame(diff),as.data.frame(counts(dds,normalize=FALSE)),by="row.names",sort=FALSE) # 合并文件

write.csv(resdata,file="diff_SR.csv") # 结果输出为.CSV文件
```

## 二、绘制热图 ##

```bash
data <- read.delim("clipboard",header=T,row.names=1) # 从剪贴板载入数据

col_anno=data.frame(PT=c("S","S","S","S","S","S","S","S","S","S","S","S","R","R","R","R","R","R","R","R","R","R","R"),row.names=colnames(data)) # 定义分组信息，具体情况具体设置，本例是对列进行分组，前12列为S，后11列为R

pheatmap(data,scale="row",annotation_col=col_anno,color=colorRampPalette(c("navy","white","firebrick3"))(50)) # 绘制热图
```