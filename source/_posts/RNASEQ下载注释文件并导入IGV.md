---
title: RNASEQ分析入门笔记6-下载注释文件并导入IGV
date: 2018-03-23 23:02:02
updated: 2018-03-23 23:02:02
tags: RNASEQ
categories: 编程语言
---

**注释文件下载流程**

进入Gencode网站（[http://www.gencodegenes.org/）](http://www.gencodegenes.org/%EF%BC%89)，data>>human>>GRCh37-mapped releases>>GENCODE release 26>>Comprehensive genome annotation/ GTF, GFF3

右键获取下载链接，使用wget命令进行下载：

``` bash
cd ncbi/hg19 #切换到hg19目录下
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_26/GRCh37_mapping/gencode.v26lift37.annotation.gtf.gz # 下载GTF文件
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_26/GRCh37_mapping/gencode.v26lift37.annotation.gff3.gz # 下载GFF3文件
```

文件下载完成后，使用gzip命令解压文件，同时删除压缩文件，

``` bash
gzip -d gencode.v26lift37.annotation.gtf.gz # 解压GTF文件
gzip -d gencode.v26lift37.annotation.gff3.gz # 解压GFF3文件
```

使用ls命令查看当前目录下得文件，

> gencode.v26lift37.annotation.gff3
gencode.v26lift37.annotation.gtf
hg19.fa (之前下载并合并的参考基因组文件)


**注释文件导入IGV流程**

IGV下载地址：http://software.broadinstitute.org/software/igv/download

加载*hg19.fa*，点击Genome/load genome from files/hg19.fa
加载*gencode.v26lift37.annotation.gtf*与*gencode.v26lift37.annotation.gff3*，这两个文件不能直接加载，需要index文件，创建index文件流程如下：
1. 点击Tools/run igvtools/command选择sort，Input File选择GTF/GFF3文件，点击run，运行完成后会产生两个文件：gencode.v26lift37.annotation.sorted.gtf
gencode.v26lift37.annotation.sorted.gff3
2. 点击Tools/run igvtools/command选择index，Input File选择 .sorted.gtf/ .sorted.gff3文件，点击run，运行完成后会产生另外两个文件：gencode.v26lift37.annotation.sorted.gtf.idx
gencode.v26lift37.annotation.sorted.gff3.idx
3. 点击Genome/load genome from files/gencode.v26lift37.annotation.sorted.gtf.idx
点击Genome/load genome from files/gencode.v26lift37.annotation.sorted.gff3.idx
加载完成。



