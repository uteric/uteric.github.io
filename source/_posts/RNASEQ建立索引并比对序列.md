---
title: RNASEQ分析入门笔记3-HISAT2建立索引并比对序列
date: 2018-03-23 22:28:24
updated: 2018-03-23 22:28:24
tags: RNASEQ
categories: 编程语言
---

**建立HISAT2索引**

    hisat2-build -p 4 hg19.fa genome

命令运行完成后，会在hg19文件夹下生成如下文件：

> genome.1.ht2
genome.2.ht2
genome.3.ht2
genome.4.ht2
genome.5.ht2
genome.6.ht2
genome.7.ht2
genome.8.ht2

建立索引文件需要大约一个小时（MAC: 2.6 GHz Intel Core i5/ 8 GB 1600 MHz DDR3），记录如下：

> Total time for call to driver() for forward index: 01:01:06

或者，也可以直接在hisat2官网下载，文件大小大约3.9GB：http://ccb.jhu.edu/software/hisat2/index.shtml

    wget ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/hg19.tar.gz
    tar -zxvf hg19.tar.gz
---

**使用HISAT2进行序列比对**

命令行目录：~
hg19 index目录：ncbi/hg19
fastq文件目录: miniconda3/fastq
输出目录：ncbi/aligned

注释：-t 记录运行时间 -x hg19 index 文件路径 -1 -2 测序的两个fastq文件 -S 比对结果输出路径

    hisat2 -t -x ncbi/hg19/genome -1 miniconda3/fastq/SRR3589956.sra_1.fastq.gz -2 miniconda3/fastq/SRR3589956.sra_2.fastq.gz -S ncbi/aligned/SRR3589956.sam

结果如下：

> Time loading forward index: 00:00:07
Time loading reference: 00:00:02
Multiseed full-index search: 00:39:18
28856780 reads; of these:
  28856780 (100.00%) were paired; of these:
    1838758 (6.37%) aligned concordantly 0 times
    24733251 (85.71%) aligned concordantly exactly 1 time
    2284771 (7.92%) aligned concordantly >1 times
    ----
    1838758 pairs aligned concordantly 0 times; of these:
      90903 (4.94%) aligned discordantly 1 time
    ----
    1747855 pairs aligned 0 times concordantly or discordantly; of these:
      3495710 mates make up the pairs; of these:
        2034757 (58.21%) aligned 0 times
        1221304 (34.94%) aligned exactly 1 time
        239649 (6.86%) aligned >1 times
96.47% overall alignment rate
Time searching: 00:39:21
Overall time: 00:39:28


此命令只单独比对SRR3589956，若要运行多个文件，则可以使用VIM新建srr.sh文件，内容如下：

    for i in `seq 57 58`
    do
        hisat2 -t -x ncbi/hg19/genome -1 miniconda3/fastq/SRR35899${i}.sra_1.fastq.gz -2 miniconda3/fastq/SRR35899${i}.sra_2.fastq.gz -S ncbi/aligned/SRR35899${i}.sam
    done

注意，新建的srr.sh文件是不能直接运行的，因为默认新建的shell文件只有读和写得权限，因此需要首先添加执行的权限，命令如下：

    chmod +x srr.sh

此时，srr.sh文件就可以执行了，在命令行目录下运行：
    
    ./srr.sh

即可开始执行多个文件的比对。

