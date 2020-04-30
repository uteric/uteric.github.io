---
title: RNASEQ分析入门笔记2-下载原始数据
date: 2018-03-21 16:41:13
updated: 2018-03-21 16:41:13
tags: RNASEQ
categories: 编程语言
---

## 获取原始数据地址 ##

参考文献：*AKAP95 regulates splicing through scaffolding RNAs and RNA processing factors. Nat Commun 2016 Nov 8;7:13347. PMID: 27824034*

根据文献，原始数据下载地址如下：ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747， 该目录下共有如下文件：

> | 26/May/2016 00:00 |    dir    | [SRR3589948](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589948/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589949](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589949/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589950](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589950/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589951](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589951/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589952](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589952/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589953](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589953/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589954](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589954/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589955](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589955/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589956](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589956/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589957](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589957/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589958](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589958/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589959](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589959/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589960](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589960/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589961](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589961/) |  |
| 26/May/2016 00:00 |    dir    | [SRR3589962](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589962/) |

我们只对最后4个来自小鼠的测序数据进行分析，即*59-62.sra

## 下载文件语法 ##

#### 单个文件下载 ####

本文以下载SRR3589959.sra为例：
``` bash
wget ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589959/SRR3589959.sra
```
下载过程如下：
> --2018-03-21 14:52:21--  ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589959/SRR3589959.sra
=> 'SRR3589959.sra'
Resolving ftp-trace.ncbi.nlm.nih.gov... 130.14.250.10, 2607:f220:41e:250::12
Connecting to ftp-trace.ncbi.nlm.nih.gov|130.14.250.10|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR3589959 ... done.
==> SIZE SRR3589959.sra ... 1321462823
==> PASV ... done.    ==> RETR SRR3589959.sra ... done.
Length: 1321462823 (1.2G) (unauthoritative)
SRR3589959.sra      100%[===================>]   1.23G  3.82MB/s    in 11m 8s
2018-03-21 15:03:31 (1.89 MB/s) - 'SRR3589959.sra' saved [1321462823]

文件大小1.2GB，耗时约11Mins

#### 批量下载文件 ####

批量依次下载SRR3589959.sra，SRR3589960.sra，SRR3589961.sra，SRR3589962.sra，四个文件，

``` bash
for i in ` seq 59 62`;
do
wget ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP/SRP075/SRP075747/SRR35899${i}/SRR35899${i}.sra  
done
```
将上述语法保存为*.sh文件，并在终端中执行即可。
