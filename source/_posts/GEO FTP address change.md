---
title: GEO数据库测序数据下载位置变更
date: 2019-12-28 18:08:45
updated: 2019-12-28 18:08:45
tags: GEO
categories: 编程语言
---


[GEO](https://www.ncbi.nlm.nih.gov/geo/)是一个公共基因数据库，包含绝大多数已发表文章的原始测序数据，在学习测序数据分析时，我们需要从GEO数据库来下载原始测序文件。

去年我在学习RNA测序分析时，写了几篇RNASEQ分析入门笔记，最近，当我再打开时，发现之前提到的SRA测序文件下载地址发生了变动，因此，特别写下这篇小小笔记以方便初学RNASEQ的朋友们少走弯路。

在这篇笔记[RNASEQ分析入门笔记2-下载原始数据](https://uteric.github.io/RNASEQ%E4%B8%8B%E8%BD%BD%E5%8E%9F%E5%A7%8B%E6%95%B0%E6%8D%AE/)中，[SRR3589959]的下载地址原本是：ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589959/SRR3589959.sra，但是，现在这个地址已经打不开了，我仔细查看了FTP文件目录，发现FTP地址发生了变化，但仍然很有规律，可以分为如下两个部分：

1）通用部分：<ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/>
2）变动部分：SRR358/SRR3589959/SRR3589959.sra #格式为SRRabc/SRRabcdef/SRRabcdef.sra

因此，SRR3589959.sra的地址即为：<ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR358/SRR3589959/SRR3589959.sra>

对于数据下载，使用wget命令即可，

```
wget -c ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR358/SRR3589959/SRR3589959.sra # -c表示断点续传，以防意外情况导致下载失败
```
