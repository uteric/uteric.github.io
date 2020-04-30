---
title: RNASEQ学习流程
date: 2018-05-21 23:33:04
updated: 2018-05-21 23:33:04
tags: RNASEQ
categories: 编程语言
---

参考文献：[Transcript-level expression analysis of RNA-seq experiments with HISAT, StringTie and Ballgown](https://link.jianshu.com/?t=http://www.nature.com/nprot/journal/v11/n9/full/nprot.2016.095.html)

---

## 一、下载数据并解压 ##

``` bash
wget ftp://ftp.ccb.jhu.edu/pub/RNAseq_protocol/chrX_data.tar.gz # 下载原始数据
tar zxvf chrX_data.tar.gz # 解压
```
解压后在当前目录下生成新文件夹chrX_data，里面包含如下文件：

drwxr-xr-x   3 xiuliliu  staff  102 Jul 12  2016 genes
drwxr-xr-x   3 xiuliliu  staff  102 Jan 14  2016 genome
-rw-r--r--   1 xiuliliu  staff  337 Jan 14  2016 geuvadis_phenodata.csv
drwxr-xr-x  10 xiuliliu  staff  340 Jan 14  2016 indexes
-rw-r--r--   1 xiuliliu  staff  228 Jan 14  2016 mergelist.txt
drwxr-xr-x  26 xiuliliu  staff  884 Jan 14  2016 samples

其中，samples中包含24个.fastq.gz文件，分为12对，从GBR和YRI两地各取6个样，男女各3个，构成基本的生物学重复。

-rw-r--r--  1 xiuliliu  staff   91425269 Jan 14  2016 ERR188044_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   91314392 Jan 14  2016 ERR188044_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   87664981 Jan 14  2016 ERR188104_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   88268450 Jan 14  2016 ERR188104_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff  112984506 Jan 14  2016 ERR188234_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff  114271070 Jan 14  2016 ERR188234_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   61157734 Jan 14  2016 ERR188245_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   62378428 Jan 14  2016 ERR188245_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   67433106 Jan 14  2016 ERR188257_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   68213815 Jan 14  2016 ERR188257_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   39876761 Jan 14  2016 ERR188273_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   40639110 Jan 14  2016 ERR188273_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   91600737 Jan 14  2016 ERR188337_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   92210453 Jan 14  2016 ERR188337_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   66033878 Jan 14  2016 ERR188383_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   65623043 Jan 14  2016 ERR188383_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   90165491 Jan 14  2016 ERR188401_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   90548373 Jan 14  2016 ERR188401_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   57960187 Jan 14  2016 ERR188428_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   58159176 Jan 14  2016 ERR188428_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   73187016 Jan 14  2016 ERR188454_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   72904430 Jan 14  2016 ERR188454_chrX_2.fastq.gz
-rw-r--r--  1 xiuliliu  staff   77647967 Jan 14  2016 ERR204916_chrX_1.fastq.gz
-rw-r--r--  1 xiuliliu  staff   78167352 Jan 14  2016 ERR204916_chrX_2.fastq.gz

---

## 二、比对读段到基因组上 (Align the RNA-seq reads to the genome) ##

### 1 | Map the reads for each sample to the reference genome ###

``` bash
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR204916_chrX_1.fastq.gz -2 chrX_data/samples/ERR204916_chrX_2.fastq.gz -S ERR204916_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188454_chrX_1.fastq.gz -2 chrX_data/samples/ERR188454_chrX_2.fastq.gz -S ERR188454_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188428_chrX_1.fastq.gz -2 chrX_data/samples/ERR188428_chrX_2.fastq.gz -S ERR188428_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188401_chrX_1.fastq.gz -2 chrX_data/samples/ERR188401_chrX_2.fastq.gz -S ERR188401_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188383_chrX_1.fastq.gz -2 chrX_data/samples/ERR188383_chrX_2.fastq.gz -S ERR188383_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188337_chrX_1.fastq.gz -2 chrX_data/samples/ERR188337_chrX_2.fastq.gz -S ERR188337_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188273_chrX_1.fastq.gz -2 chrX_data/samples/ERR188273_chrX_2.fastq.gz -S ERR188273_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188257_chrX_1.fastq.gz -2 chrX_data/samples/ERR188257_chrX_2.fastq.gz -S ERR188257_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188245_chrX_1.fastq.gz -2 chrX_data/samples/ERR188245_chrX_2.fastq.gz -S ERR188245_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188234_chrX_1.fastq.gz -2 chrX_data/samples/ERR188234_chrX_2.fastq.gz -S ERR188234_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188104_chrX_1.fastq.gz -2 chrX_data/samples/ERR188104_chrX_2.fastq.gz -S ERR188104_chrX.sam
> hisat2 -p 8 --dta -x chrX_data/indexes/chrX_tran -1 chrX_data/samples/ERR188044_chrX_1.fastq.gz -2 chrX_data/samples/ERR188044_chrX_2.fastq.gz -S ERR188044_chrX.sam
```

以ERR188044为例，比对结果如下：

1321477 reads; of these:
1321477 (100.00%) were paired; of these:
121769 (9.21%) aligned concordantly 0 times
1183443 (89.55%) aligned concordantly exactly 1 time
16265 (1.23%) aligned concordantly >1 times
----
121769 pairs aligned concordantly 0 times; of these:
4200 (3.45%) aligned discordantly 1 time
----
117569 pairs aligned 0 times concordantly or discordantly; of these:
235138 mates make up the pairs; of these:
119859 (50.97%) aligned 0 times
112782 (47.96%) aligned exactly 1 time
2497 (1.06%) aligned >1 times
95.46% overall alignment rate

比对完成后生成如下SAM文件：

-rw-r--r--  1 xiuliliu  staff   774297504 May  1 22:25 ERR188044_chrX.sam
-rw-r--r--  1 xiuliliu  staff   751836403 May  1 22:33 ERR188104_chrX.sam
-rw-r--r--  1 xiuliliu  staff   917023342 May  1 22:36 ERR188234_chrX.sam
-rw-r--r--  1 xiuliliu  staff   502043631 May  1 22:37 ERR188245_chrX.sam
-rw-r--r--  1 xiuliliu  staff   558972026 May  1 22:39 ERR188257_chrX.sam
-rw-r--r--  1 xiuliliu  staff   335808493 May  1 22:41 ERR188273_chrX.sam
-rw-r--r--  1 xiuliliu  staff   748130535 May  1 22:43 ERR188337_chrX.sam
-rw-r--r--  1 xiuliliu  staff   561738897 May  1 22:44 ERR188383_chrX.sam
-rw-r--r--  1 xiuliliu  staff   769125833 May  1 22:46 ERR188401_chrX.sam
-rw-r--r--  1 xiuliliu  staff   490343972 May  1 22:47 ERR188428_chrX.sam
-rw-r--r--  1 xiuliliu  staff   611111154 May  1 22:49 ERR188454_chrX.sam
-rw-r--r--  1 xiuliliu  staff   640234529 May  1 22:50 ERR204916_chrX.sam

---

### 2 | Sort and convert the SAM files to BAM ###

``` bash
samtools sort -@ 8 -o ERR188104_chrX.bam ERR188104_chrX.sam
samtools sort -@ 8 -o ERR188234_chrX.bam ERR188234_chrX.sam
samtools sort -@ 8 -o ERR188245_chrX.bam ERR188245_chrX.sam
samtools sort -@ 8 -o ERR188257_chrX.bam ERR188257_chrX.sam
samtools sort -@ 8 -o ERR188273_chrX.bam ERR188273_chrX.sam
samtools sort -@ 8 -o ERR188337_chrX.bam ERR188337_chrX.sam
samtools sort -@ 8 -o ERR188383_chrX.bam ERR188383_chrX.sam
samtools sort -@ 8 -o ERR188401_chrX.bam ERR188401_chrX.sam
samtools sort -@ 8 -o ERR188428_chrX.bam ERR188428_chrX.sam
samtools sort -@ 8 -o ERR188454_chrX.bam ERR188454_chrX.sam
samtools sort -@ 8 -o ERR204916_chrX.bam ERR204916_chrX.sam
```
运行完成后，生成对应的BAM文件：

-rw-r--r--  1 xiuliliu  staff   149690318 May  1 23:25 ERR188044_chrX.bam
-rw-r--r--  1 xiuliliu  staff   144503638 May  1 23:26 ERR188104_chrX.bam
-rw-r--r--  1 xiuliliu  staff   192550220 May  1 23:27 ERR188234_chrX.bam
-rw-r--r--  1 xiuliliu  staff   105587796 May  1 23:28 ERR188245_chrX.bam
-rw-r--r--  1 xiuliliu  staff   117630914 May  1 23:29 ERR188257_chrX.bam
-rw-r--r--  1 xiuliliu  staff    69857742 May  1 23:30 ERR188273_chrX.bam
-rw-r--r--  1 xiuliliu  staff   157532931 May  1 23:31 ERR188337_chrX.bam
-rw-r--r--  1 xiuliliu  staff   108256114 May  1 23:32 ERR188383_chrX.bam
-rw-r--r--  1 xiuliliu  staff   151150838 May  1 23:32 ERR188401_chrX.bam
-rw-r--r--  1 xiuliliu  staff    97383432 May  1 23:33 ERR188428_chrX.bam
-rw-r--r--  1 xiuliliu  staff   121850605 May  1 23:34 ERR188454_chrX.bam
-rw-r--r--  1 xiuliliu  staff   131891473 May  1 23:34 ERR204916_chrX.bam

---

### 3 | Assemble transcripts for each sample ###

``` bash
conda install stringtie # 安装stringtie
# [-p 8]：8个线程运行；[-G]：注释文件；[-o]：输出目录；[-l]：指定输出转录本的前缀；
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188044_chrX.gtf -l ERR188044 ERR188044_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188104_chrX.gtf -l ERR188104 ERR188104_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188234_chrX.gtf -l ERR188234 ERR188234_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188245_chrX.gtf -l ERR188245 ERR188245_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188337_chrX.gtf -l ERR188337 ERR188337_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188257_chrX.gtf -l ERR188257 ERR188257_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188273_chrX.gtf -l ERR188273 ERR188273_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188383_chrX.gtf -l ERR188383 ERR188383_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188401_chrX.gtf -l ERR188401 ERR188401_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188428_chrX.gtf -l ERR188428 ERR188428_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR188454_chrX.gtf -l ERR188454 ERR188454_chrX.bam
stringtie -p 8 -G chrX_data/genes/chrX.gtf -o ERR204916_chrX.gtf -l ERR204916 ERR204916_chrX.bam
```
运行完成后，生成如下文件：

-rw-r--r--  1 xiuliliu  staff     1911015 May  2 22:09 ERR188044_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1905249 May  2 22:12 ERR188104_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1914897 May  2 22:13 ERR188234_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1750782 May  2 22:14 ERR188245_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1705229 May  2 22:19 ERR188257_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1487570 May  2 22:19 ERR188273_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1928401 May  2 22:17 ERR188337_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1744763 May  2 22:20 ERR188383_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1936094 May  2 22:20 ERR188401_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1617807 May  2 22:21 ERR188428_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1773237 May  2 22:21 ERR188454_chrX.gtf
-rw-r--r--  1 xiuliliu  staff     1818351 May  2 22:22 ERR204916_chrX.gtf

---

### 4 | Merge transcripts from all samples ###

``` bash
# [-p]：线程数；[-G]：注释文件；[-o]：输出目录
stringtie --merge -p 8 -G chrX_data/genes/chrX.gtf -o stringtie_merged.gtf chrX_data/mergelist.txt
```

运行完成后，生成一个新的文件：

stringtie_merged.gtf

---

### 5 | Examine how the transcripts compare with the reference annotation ###

``` bash
conda install gffcompare # 安装gffcompare
ffcompare -r chrX_data/genes/chrX.gtf -G -o merged stringtie_merged.gtf
```
运行完成后，生成多个文件：

merged.annotated.gtf                 
merged.loci                 
merged.stats                 
merged.stringtie_merged.gtf.refmap
merged.stringtie_merged.gtf.tmap                 
merged.tracking

---

### 6 | Estimate transcript abundances and create table counts for Ballgown ###

``` bash
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188044/ERR188044_chrX.gtf ERR188044_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188104/ERR188104_chrX.gtf ERR188104_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188234/ERR188234_chrX.gtf ERR188234_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188245/ERR188245_chrX.gtf ERR188245_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188257/ERR188257_chrX.gtf ERR188257_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188273/ERR188273_chrX.gtf ERR188273_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188337/ERR188337_chrX.gtf ERR188337_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188383/ERR188383_chrX.gtf ERR188383_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188401/ERR188401_chrX.gtf ERR188401_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188428/ERR188428_chrX.gtf ERR188428_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR188454/ERR188454_chrX.gtf ERR188454_chrX.bam
stringtie -e -B -p 8 -G stringtie_merged.gtf -o ballgown/ERR204916/ERR204916_chrX.gtf ERR204916_chrX.bam
```

运行完成后，将生成ballgown文件夹，

drwxr-xr-x  8 xiuliliu  staff  272 May  2 22:58 ERR188044
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:00 ERR188104
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:01 ERR188234
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:01 ERR188245
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:02 ERR188257
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:02 ERR188273
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:03 ERR188337
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:03 ERR188383
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:04 ERR188401
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:04 ERR188428
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:05 ERR188454
drwxr-xr-x  8 xiuliliu  staff  272 May  2 23:06 ERR204916

以ERR188044为例，包含：

-rw-r--r--  1 xiuliliu  staff  4737232 May  2 22:58 ERR188044_chrX.gtf
-rw-r--r--  1 xiuliliu  staff   279338 May  2 22:58 e2t.ctab
-rw-r--r--  1 xiuliliu  staff   767920 May  2 22:58 e_data.ctab
-rw-r--r--  1 xiuliliu  staff   244338 May  2 22:58 i2t.ctab
-rw-r--r--  1 xiuliliu  staff   358966 May  2 22:58 i_data.ctab
-rw-r--r--  1 xiuliliu  staff   288682 May  2 22:58 t_data.ctab

---

## 三、差异分析（Differential Expression Analysis） ##

### 7 | Load relevant R packages ###

``` bash
> R
> source("http://bioconductor.org/biocLite.R")
> biocLite("ballgown")
> biocLite("dplyr")
> biocLite("devtools")
> biocLite("RSkittleBrewer") # 安装RSkittleBrewer，如果不行，尝试如下方法：
> devtools::install_github('RSkittleBrewer', 'alyssafrazee')
# 安装完成后，加载相关R包
> library(ballgown) # 分析转录组差异表达
> library(RSkittleBrewer) # 设置颜色
> library(genefilter) # 快速计算均值和方差（means & variances）
> library(dplyr) # 对结果进行分类排序
> library(devtools) # 安装R包
```
### 8 | Load the phenotype data for the samples ###

chrX_data/geuvadis_phenodata.csv中包含样品的表型数据，**行为样品，列为变量。**
``` bash
> pheno_data = read.csv("chrX_data/geuvadis_phenodata.csv")
```
| "ids" | "sex" | "population" |
|:---:|:---:|:---:|
|"ERR188044"|"male"|"YRI"|
|"ERR188104"|"male"|"YRI"|
|"ERR188234"|"female"|"YRI"|
|"ERR188245"|"female"|"GBR"|
|"ERR188257"|"male"|"GBR"|
|"ERR188273"|"female"|"YRI"|
|"ERR188337"|"female"|"GBR"|
|"ERR188383"|"male"|"GBR"|
|"ERR188401"|"male"|"GBR"|
|"ERR188428"|"female"|"GBR"|
|"ERR188454"|"male"|"YRI"|
|"ERR204916"|"female"|"YRI"|

---

### 9 | Read in expression data ###

ballgown包含三个参数：
**dataDir，**数据存储目录，本例中命名为ballgown；
**samplePattern，**样本名称的特征模式，本例中为ERR；
**pData，**上一步加载的表型数据；

``` bash
> bg_chrX = ballgown(dataDir = "ballgown", samplePattern = "ERR", pData=pheno_data)
```

### 10 | Filter to remove low-abundance genes ###

使用方差过滤掉低丰度的基因，即去掉所有样本中方差小于1的基因。

``` bash
> bg_chrX_filt = subset (bg_chrX, "rowVars(texpr(bg_chrX)) >1", genomesubset=TRUE) 
# texpr()，在ballgown中用于提取基因表达水平；
# rowVars()，计算数组中每一行的方差；
# rowVars(texpr(bg_chrX)) >1，代表提取每一行的表达数据，计算方差，并保留方差大于1的基因作为形构成子集的条件；
# subset (x, cond, genomesubset=TRUE)，x，抽取子集的对象；cond，子集的条件；genomesubset=TRUE（对象为特定基因组），FALSE（对象为特定样品）
```

---

### 11 | Identify transcripts that show statistically significant differences between groups ###

识别组间有显著差异的转录本：在此特别说明，我们需要考虑由其他变量引起的表达变化。在本例中，有两个变量，即sex & population，在计算男女性别（sex）之间的差异时，需要考虑族群（population）这一变量对结果的影响，即需要修正由族群引起的表达差异。因此，需要使用stattest这个功能。

``` bash
> results_transcripts = stattest(bg_chrX_filt, feature="transcript", covariate="sex", adjustvars = c("population"), getFC=TRUE, meas="FPKM") 
# stattest基本语法：stattest(gown = NULL, gowntable = NULL, pData = NULL, mod = NULL, mod0 = NULL, feature = c("gene", "exon", "intron", "transcript"), meas = c("cov", "FPKM", "rcount", "ucount", "mrcount", "mcov"), timecourse = FALSE, covariate = NULL, adjustvars = NULL, gexpr = NULL, df = 4, getFC = FALSE, libadjust = NULL, log = TRUE)
# feature，申明基因特征的类型，如"gene"/"transcript"/"exon"/"intron"
# covariate，协变量，感兴趣的协变量
# adjustvars，混杂因子
# getFC=TRUE，返回针对文库规模和混杂因子进行调整后的表达倍数变化
# meas，即measurement，包括"cov"/"FPKM"/"rcount"/"ucount"/"mrcount"/"mcov"
> head (results_transcripts) # 显示前6行
> tail (results_transcripts) # 显示后6行
```

输出如下：

| |feature |id  |      fc |     pval |     qval |
|:---:|:---:|:---:|:---:|:---:|:---:|
|1 |transcript | 1 |0.9705279 |0.8702505 |0.9680215|
|2 |transcript | 2 |1.8315567 |0.5824978 |0.9194471|
|3 |transcript | 3 |2.2376558 |0.5410260 |0.9174339|
|4 |transcript | 4 |0.1745325 |0.2063035 |0.8959691|
|5 |transcript | 5 |0.6220551 |0.2836476 |0.8997697|
|6 |transcript | 6 |0.6956666 |0.3957555 |0.8997697|
|...|...|...|...|...|...|
|2193| transcript | 3444 |1.5894619| 0.5488017 |0.9174339|
|2194 |transcript | 3445 |1.3396594 |0.8246823 |0.9661891|
|2195 |transcript | 3446 |0.4460239 |0.5313140 |0.9174339|
|2196 |transcript | 3447 |3.7182648 |0.2045574 |0.8959691|
|2197 |transcript | 3448 |0.5894432 |0.5519984 |0.9174339|
|2198 |transcript | 3449 |0.9296388 |0.8386784 |0.9662358|

我们看到，results_transcripts是一个2198x5的矩阵，即在男女两组间有显著差异的转录本共2198个。

---

### 12 | Identify genes that show statistically significant differences between groups ###

识别组间有显著差异的基因：同样，再次使用stattest命令，但要设置feature="gene"，

``` bash
> results_genes = stattest(bg_chrX_filt, feature="gene", covariate="sex", adjustvars = c("population"), getFC=TRUE, meas="FPKM")
> head (results_genes) 
> tail (results_genes)
```

输出如下：

| |feature |id  |      fc |     pval |     qval |
|:---:|:---:|:---:|:---:|:---:|:---:|
|1  |  gene |   MSTRG.1| 1.0842110| 0.6894045 |0.9108614|
|2  |  gene |  MSTRG.10| 0.5255349 |0.2155227 |0.7052821|
|3  |  gene | MSTRG.100| 1.1078716 |0.8361049 |0.9714996|
|...|...|...|...|...|
|983 |   gene |NR_110830| 1.2138007| 0.5460477 |0.8687917|
|984 |   gene |NR_131236 |2.0266878 |0.1630176 |0.6591538|
|985 |   gene |NR_131237| 0.8019495 |0.6388593 |0.9074799|

总计发现985个在男女两组间有显著差异的基因。

---

### 13 | Add gene names and gene IDs to the *results_transcripts* data frame ###

``` bash
> results_transcripts = data.frame (geneNames=ballgown::geneNames(bg_chrX_filt), geneIDs=ballgown::geneIDs(bg_chrX_filt), results_transcripts)
# ballgown::geneNames，代表使用ballgown包中的geneNames函数
# ballgown::geneIDs，代表使用ballgown包中的geneIDs函数
> head (results_transcripts)
> tail (results_transcripts)
```

输出如下：

|  | geneNames | geneIDs |   feature | id |       fc |     pval |     qval |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1   |   . |MSTRG.2| transcript | 1 |0.9705279 |0.8702505 |0.9680215|
|2   | PLCXD1 |MSTRG.2| transcript | 2 |1.8315567 |0.5824978 |0.9194471|
|3   |  . |MSTRG.2 |transcript | 3 |2.2376558 |0.5410260 |0.9174339|
|...|...|...|...|...|...|...|...|
|3447  |   .| MSTRG.1033 |transcript |3447 |3.7182648 |0.2045574 |0.8959691|
|3448  |    . |MSTRG.1033 |transcript |3448| 0.5894432 |0.5519984| 0.9174339|
|3449 | DDX11L16 |MSTRG.1034| transcript| 3449 |0.9296388| 0.8386784 |0.9662358|

---

### 14 | Sort the results from the smallest *P value* to the largest ###

``` bash
> results_transcripts = arrange(results_transcripts, pval)
# arrange()，是dplyr下的函数，默认状态下，按指定的列对行进行升序排列
> results_genes = arrange(results_genes, pval)
```
排序后的results_transcripts如下：

|  |geneNames |  geneIDs |   feature |  id |         fc |        pval |        qval|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1    |     .| MSTRG.503 |transcript |1623 |0.031022135 |1.600243e-10 |2.636450e-07
|2   |      . |MSTRG.503 |transcript |1621| 0.016352577 |3.101146e-10 |2.636450e-07
|3  |    XIST |MSTRG.503 |transcript| 1622| 0.003107383 |3.598430e-10 |2.636450e-07|
|...|...|...|...|...|...|...|...|
|2196  |  ARMCX5 |MSTRG.650 |transcript| 1990 |1.0009910| 0.9991690 |0.9994574|
|2197  |  MAP7D3| MSTRG.888 |transcript |2848 |1.0005197 |0.9991786 |0.9994574|
|2198   |   FHL1 |MSTRG.887 |transcript| 2844| 0.9993523 |0.9994574 |0.9994574|

排序后的results_genes如下：

| |feature |       id |         fc |        pval  |       qval|
|:---:|:---:|:---:|:---:|:---:|:---:|
|1 |   gene |MSTRG.503| 0.002374652| 2.081024e-11| 2.049809e-08|
|2 |   gene |MSTRG.502| 0.082721440| 7.853611e-06| 2.788929e-03|
|3 |   gene |MSTRG.133| 3.103423470| 1.032255e-05| 2.788929e-03|
|...|...|...|...|...|...|
|983  |  gene |   MSTRG.388| 0.9989017| 0.9977412| 0.9996474|
|984  |  gene  |   MSTRG.86| 1.0003031| 0.9995072| 0.9996474|
|985 |   gene  |  MSTRG.933 |1.0002451| 0.9996474| 0.9996474|

---

### 15 | Write the results to a csv file that can be shared and distributed ###

``` bash
> write.csv(results_transcripts, "chrX_transcripts_results.csv", row.names=FALSE)
> write.csv (results_genes, "chrX_gene_results.csv", row.names=FALSE)
```

---

### 16 | Identify transcripts and genes with a q value <0.05 ###

subset(x, ...)，返回向量、矩阵或数据框中满足条件的子集，x代表被子集的对象，...代表补充条件；

``` bash
> subset (results_transcripts, results_transcripts$qval < 0.05) # results_transcripts$qval，代表results_transcripts数据框中的变量qval
> subset (results_genes, results_genes$qval<0.05)
```

|   |geneNames|   geneIDs|    feature |  id |         fc |        pval |        qval|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1 |         . |MSTRG.503 |transcript| 1623 |0.031022135| 1.600243e-10 |2.636450e-07|
|2  |        . |MSTRG.503| transcript |1621 |0.016352577| 3.101146e-10 |2.636450e-07|
|3   |    XIST| MSTRG.503| transcript| 1622| 0.003107383| 3.598430e-10 |2.636450e-07|
|4   |      .| MSTRG.503| transcript| 1624| 0.028430569 |4.552122e-08 |2.501391e-05|
|5   |    TSIX |MSTRG.502| transcript| 1620| 0.080804059| 2.230664e-06 |9.806001e-04|
|6   |       . |MSTRG.585| transcript |1812| 7.403215751 |1.083185e-05 |3.968068e-03|
|7   |       . |MSTRG.734| transcript| 2296| 0.285432112 |4.293753e-05 |1.236355e-02|
|8   |       . |MSTRG.589| transcript| 1816| 9.079691942| 4.499928e-05 |1.236355e-02|
|9   |       . |MSTRG.133| transcript|  415| 3.282982493| 9.861143e-05 |2.260472e-02|
|10 |    KDM6A| MSTRG.242| transcript|  721| 0.056618320 |1.028422e-04 |2.260472e-02|
|11 |   PNPLA4 | MSTRG.58| transcript | 180| 0.592766066| 1.837704e-04 |3.672066e-02|

|  |feature |       id |         fc |        pval |        qval|
|:---:|:---:|:---:|:---:|:---:|:---:|
|1 |    gene| MSTRG.503| 0.002374652| 2.081024e-11| 2.049809e-08|
|2 |    gene| MSTRG.502| 0.082721440| 7.853611e-06| 2.788929e-03|
|3 |    gene| MSTRG.133| 3.103423470| 1.032255e-05| 2.788929e-03|
|4 |    gene| MSTRG.585| 7.275640247| 1.132560e-05| 2.788929e-03|
|5 |    gene| MSTRG.734| 0.274009446| 1.719431e-05| 3.387279e-03|
|6 |    gene| MSTRG.589| 9.144287534| 4.825828e-05| 7.922401e-03|
|7 |    gene| MSTRG.491| 0.638039877| 9.299543e-05| 1.308579e-02|
|8 |    gene|  MSTRG.58| 0.599529415| 1.561007e-04| 1.921990e-02|
|9|     gene| MSTRG.355| 0.628725310| 2.718713e-04| 2.975480e-02|
|10 |   gene| MSTRG.590| 7.808856957| 3.827458e-04| 3.770046e-02|
|11|    gene| MSTRG.213| 1.409064890| 4.535347e-04| 4.061197e-02|

---

## 四、绘图（Data visualization） ##

### 17 | Make the plots pretty ###

``` bash
> tropical = c('darkorange', 'dodgerblue', 'hotpink', 'limegreen', 'yellow') 
> palette (tropical) #定义调色板
```

### 18 | Show the distribution of gene abundances across samples, colored by sex ###

``` bash
> fpkm = texpr(bg_chrX, meas="FPKM") # texpr(x, meas = "FPKM")，从ballgown中抽取转录本的表达值
> fpkm = log2(fpkm+1) # +1的原因是避免出现log2(0)
> boxplot(fpkm, col=as.numeric(pheno_data$sex), las=2, ylab='log2(FPKM+1)')
# las=2，定义坐标轴标记为垂直，默认las=1，即水平；ylab，定义y轴；
```
fpkm如下：

| |FPKM.ERR188044| FPKM.ERR188104 |FPKM.ERR188234 |FPKM.ERR188245 |FPKM.ERR188257|FPKM.ERR188273 |FPKM.ERR188337 |FPKM.ERR188383 |FPKM.ERR188401|FPKM.ERR188428|FPKM.ERR188454 |FPKM.ERR204916|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|1   |    4.658196 |      4.287118      | 5.348706 |      3.878832   |4.730237 |  4.636182 |      4.997749 |      4.442006  |     4.861568       |4.700369| 4.872868   |    4.410593|
|2   |    0.000000    |   0.000000 |      4.849373    |   0.000000     |5.523984 |      0.000000     |  0.000000     |  0.000000 |      4.747225       |0.000000 |0.000000 |      0.000000|
|3   |    4.750170    |   0.000000 |      5.121661    |   0.000000       |0.000000| 0.000000 |      4.345735  |   5.597809 |      5.524776       |5.223788| 5.967379  |     0.000000|
|...|...|...|...|...|...|...|...|...|...|...|...|...|
|3447     |  0.000000     |  0.000000 |     0.0000000  |     0.000000      |7.0420943 |0.0000000  |     0.000000 |      0.000000|       0.000000      |0.0000000 |3.699866   |    0.000000|
|3448     |  0.000000      | 0.000000     | 0.0000000     |  0.000000      |5.2154062| 0.0000000    |   6.646315 |      0.000000  |     0.000000      |0.0000000 |0.000000     |  0.000000|
|3449     |  1.007382       |1.979173    |  1.7364404    |   1.343544      |0.6354767  |0.0000000     |  1.047157|       1.739338 |      0.000000      |0.0000000 |0.000000       |0.000000|

![image.png](https://upload-images.jianshu.io/upload_images/4189810-c5ff5c5b0ea9d2c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 19 | Make plots of individual transcripts across samples ###

``` bash
> ballgown::transcriptNames(bg_chrX)[9] # 第九个转录本的名称
# 9
# "NM_012227"
> ballgown::geneNames(bg_chrX)[9] # 第九个基因的名称
# 9
# "GTPBP6"
> plot (fpkm[9,] ~ pheno_data$sex, border=c(1,2), main=paste(ballgown::geneNames(bg_chrX)[9],':',ballgown::transcriptNames(bg_chrX)[9]), pch=19, xlab="Sex", ylab='log2(FPKM+1)')
> points(fpkm[9,] ~ jitter(as.numeric(pheno_data$sex)), col=as.numeric(pheno_data$sex))
```

![](https://upload-images.jianshu.io/upload_images/4189810-b64fa21011cd51e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 20 | Plot the structure and expression levels in a sample of all transcripts that share the same gene locus ###

``` bash
> plotTranscripts(ballgown::geneIDs(bg_chrX)[1622], bg_chrX, main=c('Gene XIST in sample ERR188234'), sample=c('ERR188234'))
```

![image.png](https://upload-images.jianshu.io/upload_images/4189810-17b30d74aeb41597.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 21 | Plot the average expression levels for all transcripts of a gene within different groups ###

``` bash
> plotMeans ('MSTRG.56', bg_chrX_filt, groupvar="sex", legend=FALSE)
```

![image.png](https://upload-images.jianshu.io/upload_images/4189810-db5a8b809b1ef072.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




