---
title: RNASEQ分析入门笔记10-clusterProfiler富集分析
date: 2018-09-24 21:11:12
updated: 2018-09-24 21:11:12
tags: RNASEQ
categories: 编程语言
---

## 一、安装clusterProfiler

> source("https://bioconductor.org/biocLite.R") # 如果报错，请尝试http://
biocLite("clusterProfiler")
library(clusterProfiler) # 载入包，测试是否安装成功，如果提示找不到某个包，则安装相关的包即可，比如我的电脑提示“**不存在叫‘rvcheck’这个名字的程辑包**”，则使用如下语法安装“rvcheck”，如缺乏其他包，也是同理。
> install.packages("rvcheck")

## 二、安装基因组注释数据

目前，[Bioconductor](http://bioconductor.org/packages/release/BiocViews.html#___OrgDb)官网支持19种生物的注释数据，可以直接安装：

> biocLite("org.Hs.eg.db") # 安装人类的注释数据
library(org.Hs.eg.db) # 测试，载入人类的注释数据
biocLite("org.Mm.eg.db") # 安装小鼠的注释数据
library(org.Mm.eg.db) # 测试，载入小鼠的注释数据

## 三、载入基因列表 ##

```bash
data <- read.csv("top50.csv") # 从文件中载入数据，并赋值给data
# 我们需要的是第二列的基因列表，因此，只筛选第二列即可
gene <- data[,2] # 抽取第二列，赋值给gene
gene <- bitr(gene,fromType="SYMBOL",toType="ENTREZID",OrgDb=org.Hs.eg.db) # groupGO默认使用"ENTREZID"作为输入数据，因此首先将“SYMBOL”转化为“ENTREZID”，以防出错
gene <- gene[,2] # 抽取第二列作为input
```

## 四、groupGO分析 ##

```bash
ggo <- groupGO(gene=gene,OrgDb=org.Hs.eg.db,ont="BP",level=3,readable=TRUE)
ggo <- groupGO(gene=gene,OrgDb=org.Hs.eg.db,ont="CC",level=3,readable=TRUE)
ggo <- groupGO(gene=gene,OrgDb=org.Hs.eg.db,ont="MF",level=3,readable=TRUE)
```

groupGO命令的格式为：*groupGO(gene,OrgDb,ont,level,readable)*

**gene**，指定前景基因文件，即要分析的基因集合，默认为"ENTREZID"格式；
**OrgDb**，指定基因组注释数据；
**ont**，即ontology，参数有三个，分别为CC，BP和MF，对应：细胞组分（cellular component，CC），生物过程（biological process，BP），分子功能（molecular function，MF）；
**level**，指定分析的GO水平；
**readable**，逻辑值，TRUE或FALSE；

```bash
head(as.data.frame(ggo)) # 查看前六行，结果是六行五列的数据
```

ID                              Description Count
GO:0019953 GO:0019953                      sexual reproduction     7
GO:0019954 GO:0019954                     asexual reproduction     0
GO:0022414 GO:0022414                     reproductive process    26
GO:0032504 GO:0032504      multicellular organism reproduction     9
GO:0032505 GO:0032505 reproduction of a single-celled organism     0
GO:0061887 GO:0061887         reproduction of symbiont in host     0
GeneRatio
GO:0019953     7/298
GO:0019954     0/298
GO:0022414    26/298
GO:0032504     9/298
GO:0032505     0/298
GO:0061887     0/298
geneID
GO:0019953                                                                                                                 APOB/ABAT/ANG/AR/AVPR1A/LEPR/ACOX1
GO:0019954                                                                                                                                                   
GO:0022414 APOB/AGT/AMBP/RBP4/ABAT/SERPINF1/ANG/HNF4A/AR/AVPR1A/ARG1/LEPR/GJB1/BOK/DHODH/SLC38A3/PDGFRA/TBX3/ADGRG1/IGFBP2/TTPA/ACOX1/DBH/ETNK2/NAMPT/HSD17B2
GO:0032504                                                                                                  APOB/SERPINF1/ANG/AR/AVPR1A/ARG1/PDGFRA/ACOX1/DBH
GO:0032505                                                                                                                                                   
GO:0061887                                                                                           

## 五、groupGo结果可视化 ##

```bash
barplot(ggo,drop=TRUE,showCategory=20)
```
![ggo barplot.jpeg](https://upload-images.jianshu.io/upload_images/4189810-f92d7ebc27bfe4e3.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 六、enrichGO分析 ##

```bash
ego <- enrichGO(gene=gene,universe=names(geneList),OrgDb=org.Hs.eg.db,ont="BP",pvalueCutoff=0.01,qvalueCutoff=0.05,readable=TRUE)
ego <- enrichGO(gene=gene,universe=names(geneList),OrgDb=org.Hs.eg.db,ont="CC",pvalueCutoff=0.01,qvalueCutoff=0.05,readable=TRUE)
ego <- enrichGO(gene=gene,universe=names(geneList),OrgDb=org.Hs.eg.db,ont="MF",pvalueCutoff=0.01,qvalueCutoff=0.05,readable=TRUE)
```

enrichGO命令语法：*enrichGO(gene,universe,OrgDb,ont,pvalueCutoff,qvalueCutoff,readable)*

universe，背景基因文件，可以使用默认值；

```bash
head(as.data.frame(ego))
```

ID                                   Description
GO:0072562 GO:0072562                           blood microparticle
GO:0042627 GO:0042627                                   chylomicron
GO:0031093 GO:0031093                  platelet alpha granule lumen
GO:0005788 GO:0005788                   endoplasmic reticulum lumen
GO:0034361 GO:0034361         very-low-density lipoprotein particle
GO:0034385 GO:0034385 triglyceride-rich plasma lipoprotein particle
GeneRatio   BgRatio       pvalue     p.adjust       qvalue
GO:0072562    33/288 101/11817 9.367691e-29 2.519909e-26 2.336992e-26
GO:0042627     8/288  11/11817 1.750866e-11 2.354915e-09 2.183975e-09
GO:0031093    14/288  62/11817 1.938754e-10 1.738416e-08 1.612227e-08
GO:0005788    26/288 247/11817 3.097572e-10 2.083117e-08 1.931907e-08
GO:0034361     8/288  16/11817 1.227828e-09 5.504763e-08 5.105180e-08
GO:0034385     8/288  16/11817 1.227828e-09 5.504763e-08 5.105180e-08
geneID
GO:0072562 ITIH4/ALB/FGB/FGA/FGG/ORM1/HPX/HP/VTN/APOA2/AGT/AMBP/SERPINC1/F2/C3/C9/SERPING1/HRG/ITIH1/SERPINA3/AHSG/GC/APOA4/PLG/KNG1/TF/C4BPA/C8G/C8A/APOA1/HPR/ITIH2/C1S
GO:0042627                                                                                                                    APOB/APOC3/APOA2/APOA4/APOH/APOA1/LSR/APOC1
GO:0031093                                                                                       ALB/FGB/FGA/FGG/ORM1/SERPINA1/SERPING1/HRG/SERPINA3/AHSG/F5/PLG/KNG1/VWF
GO:0005788                           APOB/ALB/FGA/FGG/SERPINA1/VTN/APOA2/SERPINC1/F2/C3/IGFBP1/AHSG/F5/SERPIND1/APOA4/KNG1/TF/PROC/APOA1/F9/ITIH2/MTTP/F10/CES2/H6PD/CDH2
GO:0034361                                                                                                                    APOB/APOC3/APOA2/APOA4/APOH/APOA1/LSR/APOC1
GO:0034385                                                                                                                    APOB/APOC3/APOA2/APOA4/APOH/APOA1/LSR/APOC1
Count
GO:0072562    33
GO:0042627     8
GO:0031093    14
GO:0005788    26
GO:0034361     8
GO:0034385     8

## 七、enrichGO可视化 ##

```bash
barplot(ego,drop=TRUE,showCategory=20)
```
![ego barplot.jpeg](https://upload-images.jianshu.io/upload_images/4189810-74b5709dbbde256d.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```bash
dotplot(ego,title="enrichGo CC dotplot")
```
![ego cc dotplot.jpeg](https://upload-images.jianshu.io/upload_images/4189810-d804a07f00df5610.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此外，还可以作enrichMap或cnetplot，命令如下：

```bash
enrichMap(ego, vertex.label.cex=1.2, layout=igraph::layout.kamada.kawai)
cnetplot(ego, foldChange=geneList)
```
