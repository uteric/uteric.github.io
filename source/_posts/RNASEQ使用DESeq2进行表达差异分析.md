---
title: RNASEQ分析入门笔记8-使用DESeq2进行表达差异分析
date: 2018-04-05 23:11:52
updated: 2018-04-05 23:11:52
tags: RNASEQ
categories: 编程语言
---

基因的差异表达分析，通常使用R中的软件包，包括：DESeq2，edgeR，limma等，今天介绍DESeq2的分析流程：

**1、在R中安装DESeq2软件包**

``` bash
source ("http://bioconductor.org/biocLite.R") # 调入安装工具Bioconductor
biocLite("DESeq2") # 安装DESeq2
library("DESeq2") # 测试是否安装成功
```
如果安装出错，请移步前文：《[MAC安装DESeq2报错及解决方案](https://uteric.github.io/MAC%E5%AE%89%E8%A3%85DESeq2%E6%8A%A5%E9%94%99%E5%8F%8A%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88/)》，作为一个生信小白，我花了3天时间才把安装DESeq2的问题解决，一路全是坑啊！

---

**2、导入分析数据**

我们还是使用《[RNASEQ分析入门笔记7-HTSeq定量基因表达水平](https://uteric.github.io/RNASEQ%20HTSeq%E5%AE%9A%E9%87%8F%E5%9F%BA%E5%9B%A0%E8%A1%A8%E8%BE%BE%E6%B0%B4%E5%B9%B3/)》中得到的数据进行分析，首先回顾一下：

上文中，我们得到了raw_count_filt2矩阵，格式如下（执行：tail (raw_count_filt2, n=5):

| |control1 | control2 | rep1 | rep2 |
|:------:|:------:|:------:|:------:|:------:|
|ENSMUSG00000110420  |      0 |       0 |   0 |   0|
|ENSMUSG00000110421  |     0  |      0  |  0 |   1|
|ENSMUSG00000110422  |    1   |   0  |  1  |  2|
|ENSMUSG00000110423  |    0   |    0  |  0  |  0|
|ENSMUSG00000110424  |   26  |     20 |  48 |  24|

可以看到，该矩阵实际只有4列，是由整数组成的，而最前面的一列是行名，可以直接从保存的.csv文件中导入：

``` bash
raw_count_filt2 <- read.csv ("raw_count_filt2.csv")  # 从保存的.csv文件导入数据，并赋值给raw_count_filt2
```

---

**3、加载DESeq2并设置样品信息**

``` bash
library(DESeq2) # 加载DESeq2包
countData <- raw_count_filt2 # 表达矩阵
condition <- factor(c("control","control","KD","KD")) # 定义condition
colData <- data.frame(row.names=colnames(countData), condition) # 样品信息矩阵
```
condition：
> [1] control control KD      KD
Levels: KD control

colData：

| | condition |
|:------:|:------:|
|control1|   control |
|control2 |  control |
|rep1     |       KD |
|rep2      |      KD |

---

**4、构建dds矩阵**

``` bash
dds <- DESeqDataSetFromMatrix(countData, DataFrame(condition), design= ~ condition ) # 构建dds矩阵
head(dds) # 查看dds矩阵的前6行
```

> class: DESeqDataSet
dim: 6 4
metadata(1): version
assays(1): counts
rownames(6): ENSMUSG00000000001 ENSMUSG00000000003 ...
ENSMUSG00000000037 ENSMUSG00000000049
rowData names(0):
colnames(4): control1 control2 rep1 rep2
colData names(1): condition

``` bash
dds2 <- DESeq(dds) # 对dds进行Normalize
```
运行成功会有如下提示：
> estimating size factors
estimating dispersions
gene-wise dispersion estimates
mean-dispersion relationship
final dispersion estimates
fitting model and testing

``` bash
resultsNames（dds) # 查看结果的名称
```

> [1] "Intercept"               "condition_control_vs_KD"

``` bash
res <- results(dds) # 使用results()函数获取结果，并赋值给res
head (res, n=5) # 查看res矩阵的前5行
```
输出的res矩阵有6列，分别是：baseMean，log2FoldChange，lfcSE，pvalue，padj：

> log2 fold change (MLE): condition control vs KD
Wald test p-value: condition control vs KD
DataFrame with 5 rows and 6 columns

|  | baseMean | log2FoldChange |    lfcSE |      stat |    pvalue | padj |
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
| | <numeric>  |    <numeric> |<numeric>|  <numeric> | <numeric>|<numeric>|
| E..0001 | 2359.75391 |    -0.08461265 | 0.2081281 | -0.4065413 | 0.68434494 | 0.9040654| 
| E..0003  |   0.00000 |             NA  |       NA |         NA         | NA | NA| 
| E..0028 | 1027.82363 |     0.03638541 | 0.2036829  | 0.1786375 | 0.85822232 | 0.9618582| 
| E..0031 |   65.37333 |     0.94588315 | 0.4428518  | 2.1358912 | 0.03268828 | 0.2511131| 
| E..0037 |   69.75843 |     0.11587980 | 0.4400431  | 0.2633374 | 0.79229054 | 0.9422796| 

``` bash
mcols(res,use.names= TRUE) # 查看res矩阵每一列的含义
```
> DataFrame with 6 rows and 2 columns

|   |                     type               |                      description |
|:------:|:------:|:------:|
|    |                <character>             |                        <character> |
| baseMean  |      intermediate  |      mean of normalized counts for all samples | 
|  log2FoldChange |      results |  log2 fold change (MLE): condition control vs KD | 
| lfcSE       |         results      |    standard error: condition control vs KD | 
| stat        |         results     |     Wald statistic: condition control vs KD | 
| pvalue |              results    |   Wald test p-value: condition control vs KD | 
| padj |                 results     |                        BH adjusted p-values | 

``` bash
summary(res) # 对res矩阵进行总结
```

> out of 28335 with nonzero total read count
adjusted p-value < 0.1
LFC > 0 (up)     : 445, 1.6%
LFC < 0 (down)   : 625, 2.2%
outliers [1]     : 0, 0%
low counts [2]   : 12683, 45%
(mean count < 18)
[1] see 'cooksCutoff' argument of ?results
[2] see 'independentFiltering' argument of ?results

其中，445个基因表达上调，625个基因表达下降，可靠程度是：p-value < 0.1

---

**5、提取差异分析结果**

``` bash
table(res$padj<0.05) # 取padj小于0.05的数据，得到743行
```
| FALSE | TRUE |
|:------:|:------:|
| 14909 |  743 |

``` bash
res <- res[order(res$padj),] # 按照padj的大小将res重新排列
diff_gene_deseq2 <- subset(res,padj < 0.05 & (log2FoldChange >1 | log2FoldChange < -1)) # 获取padj小于0.05，表达倍数取以2为对数后绝对值大于1的差异表达基因，赋值给diff_gene_deseq2
head (diff_gene_deseq2, n=5) # 查看diff_gene_deseq2矩阵的前5行
```
> log2 fold change (MLE): condition control vs KD
Wald test p-value: condition control vs KD
DataFrame with 5 rows and 6 columns

|  | baseMean | log2FoldChange |     lfcSE |      stat |       pvalue | padj | 
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|  | <numeric>  |     <numeric> | <numeric> | <numeric> |    <numeric> | <numeric> | 
| E..3309  | 548.1926 |       3.231612 | 0.2658125 | 12.157487 | 5.234431e-34 | 8.192931e-30 |
| E..6323  | 404.1894   |     3.067051 | 0.2628220 | 11.669689 | 1.820880e-31 | 1.425021e-27|
| E..1123 |   341.8542    |    2.797485 | 0.2766499 | 10.112006 | 4.887336e-24 |  2.549886e-20 |
| E..8569 |  485.4839    |    3.136032 | 0.3312999  | 9.465839 | 2.912140e-21 | 9.116163e-18 |
| E..3906 |  951.9460   |     2.382308 | 0.2510718  | 9.488553 | 2.342631e-21 | 9.116163e-18 |

``` bash
diff_gene_deseq2 <- row.names(diff_gene_deseq2) # 提取diff_gene_deseq2的行名
head (diff_gene_deseq2, n=5)
```

> [1] "ENSMUSG00000003309"
[2] "ENSMUSG00000046323" 
[3] "ENSMUSG00000001123"
[4] "ENSMUSG00000018569" 
[5] "ENSMUSG00000023906"

``` bash
resdata <- merge (as.data.frame(res),as.data.frame(counts(dds,normalize=TRUE)),by="row.names",sort=FALSE)
head (resdata,n=5)
```
| | Row.names | baseMean | log2FC |     lfcSE |      stat |        pvalue | padj |  control1 |  control2 |      rep1 |      rep2 | 
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1 | E..3309 | 548.1926 |        3.231612 | 0.2658125 | 12.157487 | 5.234431e-34 |  8.192931e-30 | 1073.9977  | 908.7239 | 125.93083  | 84.11803 | 
| 2 | E..6323 | 404.1894  |      3.067051 | 0.2628220 | 11.669689 | 1.820880e-31 | 1.425021e-27 |  723.4828 |  721.5049 |  97.10329  | 74.66657 | 
| 3 | E..1123 | 341.8542   |     2.797485 | 0.2766499 | 10.112006 | 4.887336e-24 | 2.549886e-20 |  679.8243 |  515.6735 |  84.96538 |  86.95347 | 
| 4 | E..8569 | 485.4839   |     3.136032 | 0.3312999  | 9.465839 | 2.912140e-21 |  9.116163e-18 | 1113.9140 |  630.6325 | 127.44807 |  69.94083 | 
| 5 | E..3906 | 951.9460   |     2.382308 | 0.2510718  | 9.488553 | 2.342631e-21 | 9.116163e-18 | 1862.3445 | 1333.5250 | 351.99943 | 259.91526 | 

``` bash
write.csv(resdata, file="control_vs_akap95.csv") # 将结果写入control_vs_akap95.csv文件
```



