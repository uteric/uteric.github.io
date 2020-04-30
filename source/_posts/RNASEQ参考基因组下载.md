---
title: RNASEQ分析入门笔记5-参考基因组下载
date: 2018-03-20 22:46:13
updated: 2018-03-20 22:46:13
tags: RNASEQ
categories: 编程语言
---

进入**UCSC**网站（[http://genome.ucsc.edu/](http://genome.ucsc.edu/)  ），Downloads>>Genome Data>>Human>>Feb. 2009 (GRCh37/hg19)>>full data set>>chromFa.tar.gz，右键复制链接地址如下：
http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz

下载参考基因组的代码如下：

``` bash
mkdir hg19 #在ncbi目录下新建hg19文件夹
cd hg19 #切换到ncbi/hg19目录
wget http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz #下载GRCh37/hg19，文件大约905M
tar -zxvf  chromFa.tar.gz #解压文件
```
解压后出现一大堆文件：
> chr1.fa                  chr5.fa                  chrUn_gl000223.fa
chr10.fa                 chr6.fa                  chrUn_gl000224.fa
chr11.fa                 chr6_apd_hap1.fa         chrUn_gl000225.fa
chr11_gl000202_random.fa chr6_cox_hap2.fa         chrUn_gl000226.fa
chr12.fa                 chr6_dbb_hap3.fa         chrUn_gl000227.fa
chr13.fa                 chr6_mann_hap4.fa        chrUn_gl000228.fa
chr14.fa                 chr6_mcf_hap5.fa         chrUn_gl000229.fa
chr15.fa                 chr6_qbl_hap6.fa         chrUn_gl000230.fa
chr16.fa                 chr6_ssto_hap7.fa        chrUn_gl000231.fa
chr17.fa                 chr7.fa                  chrUn_gl000232.fa
chr17_ctg5_hap1.fa       chr7_gl000195_random.fa  chrUn_gl000233.fa
chr17_gl000203_random.fa chr8.fa                  chrUn_gl000234.fa
chr17_gl000204_random.fa chr8_gl000196_random.fa  chrUn_gl000235.fa
chr17_gl000205_random.fa chr8_gl000197_random.fa  chrUn_gl000236.fa
chr17_gl000206_random.fa chr9.fa                  chrUn_gl000237.fa
chr18.fa                 chr9_gl000198_random.fa  chrUn_gl000238.fa
chr18_gl000207_random.fa chr9_gl000199_random.fa  chrUn_gl000239.fa
chr19.fa                 chr9_gl000200_random.fa  chrUn_gl000240.fa
chr19_gl000208_random.fa chr9_gl000201_random.fa  chrUn_gl000241.fa
chr19_gl000209_random.fa chrM.fa                  chrUn_gl000242.fa
chr1_gl000191_random.fa  chrUn_gl000211.fa        chrUn_gl000243.fa
chr1_gl000192_random.fa  chrUn_gl000212.fa        chrUn_gl000244.fa
chr2.fa                  chrUn_gl000213.fa        chrUn_gl000245.fa
chr20.fa                 chrUn_gl000214.fa        chrUn_gl000246.fa
chr21.fa                 chrUn_gl000215.fa        chrUn_gl000247.fa
chr21_gl000210_random.fa chrUn_gl000216.fa        chrUn_gl000248.fa
chr22.fa                 chrUn_gl000217.fa        chrUn_gl000249.fa
chr3.fa                  chrUn_gl000218.fa        chrX.fa
chr4.fa                  chrUn_gl000219.fa        chrY.fa
chr4_ctg9_hap1.fa        chrUn_gl000220.fa        chromFa.tar.gz
chr4_gl000193_random.fa  chrUn_gl000221.fa
chr4_gl000194_random.fa  chrUn_gl000222.fa

仔细观察这些解压出来的新文件，就会发现全部都是同一类型，chr*.fa，这为我们合并文件提供了极大的方便，代码如下：

cat *.fa>hg19.fa #将当前文件夹下所有.fa结尾的文件合并为一个文件，即hg19.fa

删除其他无关的解压文件，以及参考基因组压缩文件，以节省空间，

``` bash
rm chr*.fa #删除chr*.fa格式的所有文件
rm chromFa.tar.gz #删除下载的参考基因组压缩文件
ls -l #显示当前目录下得文件以及属性
```

结果如下，只有一个文件，即hg19.fa，

> -rw-r--r--  1 Administrator  staff  3199905909 Mar 16 23:07 hg19.fa

到此为止，参考基因组就准备好了。
