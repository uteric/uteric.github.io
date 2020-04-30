---
title: RNASEQ分析入门笔记7- HTSeq定量基因表达水平
date: 2018-03-27 22:24:05
updated: 2018-03-27 22:24:05
tags: RNASEQ
categories: 编程语言
---

## HTseq统计基因reads数 ##

**HTseq**是用于分析基因表达量的软件，能够根据基因结构注释GTF文件对排序过的SAM/BAM文件进行基因reads数的统计。

*语法：htseq-count [options]*

``` bash
mkdir -p ncbi/matrix/ 在ncbi目录下创建matrix文件夹，用于存放HTseq生成的.count文件。-p (-parents)，加上此选项后，系统将自动建立那些尚不存在的目录。
```
运行HTseq-count命令：

``` bash
htseq-count ~/ncbi/aligned/SRR3589959_count.sam ~/ncbi/reference/mm10/gencode.vM10.annotation.gtf > ~/ncbi/matrix/SRR3589959.count
htseq-count ~/ncbi/aligned/SRR3589960_count.sam ~/ncbi/reference/mm10/gencode.vM10.annotation.gtf > ~/ncbi/matrix/SRR3589960.count
htseq-count ~/ncbi/aligned/SRR3589961_count.sam ~/ncbi/reference/mm10/gencode.vM10.annotation.gtf > ~/ncbi/matrix/SRR3589961.count
htseq-count ~/ncbi/aligned/SRR3589962_count.sam ~/ncbi/reference/mm10/gencode.vM10.annotation.gtf > ~/ncbi/matrix/SRR3589962.count
```
上述语法是将排序过的SAM文件（count.sam）进行reads数统计，并输出为.count文件（不用担心存储空间问题，每个文件仅1M左右）。此处，使用了绝对路径，也可以根据自己的情况选择使用相对路径。

为节省时间，可以将上述命令在4个终端中同时执行，大约需要2个小时左右可以完成，生成4个文件：

> SRR3589959.count
SRR3589960.count
SRR3589961.count
SRR3589962.count

简单介绍一下.count文件的格式，在命令行下查看：

``` bash
cd ~/ncbi/matrix # 切换到.count文件所在的matrix目录
head -n 10 SRR3589959.count # 使用head命令查看SRR3589959.count文件的前10行
```
输出如下：
> ENSMUSG00000000001.4	1648
ENSMUSG00000000003.15	0
ENSMUSG00000000028.14	835
ENSMUSG00000000031.15	65
ENSMUSG00000000037.16	70
ENSMUSG00000000049.11	0
ENSMUSG00000000056.7 366
ENSMUSG00000000058.6	7
ENSMUSG00000000078.6	558
ENSMUSG00000000085.16	286

**.count**文件分为2列，第一列是基因名称（ENSMUSG00000000001.4），第二列是reads数（1648），由此可见，htseq-count命令的目的是将排序过的SAM文件（001，003，0028，0031，0037......）逐个统计reads数，但是，此时生成的.count文件只有两列。

因为所有的.count文件都排过序，并且共享相同的基因名称，所以下一步是将所有的.count文件合并起来，生成一个统一的文件，需要使用R语言脚本将4个.count文件合并为一个表达矩阵。

## R语言脚本合并表达矩阵 ##

``` bash
R # 在命令行下输入R，进入R环境
options(stringsAsFactors = FALSE) #默认为TRUE，不要辨认为factors

# 59，61为对照组，60，62为处理组，首先将四个文件分别赋值为：control1，control2，rep1，rep2
control1 <- read.table("/Users/Administrator/ncbi/matrix/SRR3589959.count", sep="\t", col.names = c("gene_id","control1"))
control2 <- read.table("/Users/Administrator/ncbi/matrix/SRR3589961.count", sep="\t", col.names = c("gene_id","control2"))
rep1 <- read.table("/Users/Administrator/ncbi/matrix/SRR3589960.count", sep="\t", col.names = c("gene_id","rep1"))
rep2 <- read.table("/Users/Administrator/ncbi/matrix/SRR3589962.count", sep="\t",col.names = c("gene_id","rep2"))

# 将4个矩阵文件按照gene_id进行合并，首先组内合并，接着组间合并，并且赋值给raw_count
raw_count <- merge(merge(control1, control2, by="gene_id"), merge(rep1,rep2, by="gene_id"))
```
对raw_count进行赋值后，我们可以查看生成的合并矩阵：

```bash
raw_count # 查看合并矩阵
```
但是raw_count文件太长，只能输出19999行，显示达到最大输出量：
> [ reached getOption("max.print") -- omitted 28826 rows ]

因此，我们只显示前10行数据，来查看一下生成的矩阵的格式：

``` bash
head (raw_count, n=10) #查看raw_count的前10行
```
输出结果如下：

>  gene_id control1 control2 rep1 rep2
1   ENSMUSG00000000001.4     1648     2306 2941 2780
2  ENSMUSG00000000003.15        0        0    0    0
3  ENSMUSG00000000028.14      835      950 1366 1051
4  ENSMUSG00000000031.15       65       83   52   53
5  ENSMUSG00000000037.16       70       53   94   66
6  ENSMUSG00000000049.11        0        3    4    5
7   ENSMUSG00000000056.7      366      386  722  301
8   ENSMUSG00000000058.6        7       11    7    3
9   ENSMUSG00000000078.6      558      896 1265 1342
10 ENSMUSG00000000085.16      286      313  343  348

合并后的矩阵共有5列，第一列是基因名称，其余四列分别是control1，control2，rep1，rep2，从上表中可以很直观地看到每个基因的表达情况。

我们再来看看raw_count的最后5行，

``` bash
tail (raw_count, n=5)
```
输出如下：
> gene_id control1 control2     rep1     rep2
48821 __alignment_not_unique 11278964 14080481 18440618 14495860
48822            __ambiguous   295498   425708   498973   381237
48823           __no_feature 12642161 15042888 22357626 18675857
48824          __not_aligned  1025857  1067038  1675660  1529309
48825        __too_low_aQual  1107674  1303141  1799176  1542845

这5行（48821， 48822， 48823， 48824， 48825）没有匹配到相应的基因，需要删除，

``` bash
raw_count_filt <- raw_count[-48821:-48825, ] # 删除5行，重新赋值给raw_count_filt
```
查看新矩阵raw_count_filt的最后5行，

``` bash
tail (raw_count_filt, n=5)
```
可以看到，48821：48825已经被删除了，

> gene_id control1 control2 rep1 rep2
48816 ENSMUSG00000110420.1        0        0    0    0
48817 ENSMUSG00000110421.1        0        0    0    1
48818 ENSMUSG00000110422.1        1        0    1    2
48819 ENSMUSG00000110423.1        0        0    0    0
48820 ENSMUSG00000110424.1       26       20   48   24

以行48820为例，第一列是基因名称ENSMUSG00000110424.1，但是这个名称在[EBI](https://www.ebi.ac.uk/)数据库上是不能直接检索到的，只能是整数形式的ENSMUSG00000110424，因此，需要进一步替换为整数的形式，并且赋值给ENSEMBL。

``` bash
ENSEMBL <- gsub("\\.\\d*", "", raw_count_filt$gene_id)
tail (ENSEMBL, n=5) # 查看ENSEMBL的最后5行
```
输出如下：
> [1] "ENSMUSG00000110420" "ENSMUSG00000110421" "ENSMUSG00000110422"
[4] "ENSMUSG00000110423" "ENSMUSG00000110424"

ENSEMBL只有一列，即修改为整数的基因名称，下一步再将ENSEMBL添加到raw_count_filt矩阵:

``` bash
row.names(raw_count_filt) <- ENSEMBL # 添加ENSENBL到raw_count_filt矩阵
tail (raw_count_filt, n=5) # 查看raw_count_filt的最后5行
```
> gene_id control1 control2 rep1 rep2
ENSMUSG00000110420 ENSMUSG00000110420.1        0        0    0    0
ENSMUSG00000110421 ENSMUSG00000110421.1        0        0    0    1
ENSMUSG00000110422 ENSMUSG00000110422.1        1        0    1    2
ENSMUSG00000110423 ENSMUSG00000110423.1        0        0    0    0
ENSMUSG00000110424 ENSMUSG00000110424.1       26       20   48   24

[optional] 这一步选做，即把raw_count_filt矩阵的第二列删掉，并赋值给raw_count_filt2：

``` bash
raw_count_filt2 <- raw_count_filt [ ,-1] # 删除raw_count_filt的第二列
tail (raw_count_filt2, n=5) # 查看raw_count_filt2最后5行
```
> control1 control2 rep1 rep2
ENSMUSG00000110420        0        0    0    0
ENSMUSG00000110421        0        0    0    1
ENSMUSG00000110422        1        0    1    2
ENSMUSG00000110423        0        0    0    0
ENSMUSG00000110424       26       20   48   24

下面，我们就可以检查一些基因的表达情况了，比如GAPDH，首先，可以在[uniprot](http://www.uniprot.org/)或者[Ensembl](http://useast.ensembl.org/index.html)数据库中找到GAPDH对应的基因编号，即ENSMUSG00000057666，并将该基因编号赋值给GAPDH：

``` bash
GAPDH <- raw_count_filt2 [rownames(raw_count_filt2)=="ENSMUSG00000057666",] # 赋值给GAPDH
GAPDH # 查看GAPDH表达情况
```

输出如下：
> control1 control2 rep1 rep2
ENSMUSG00000057666     4136     4884 7200 4929

如要查询其他基因的表达情况，以此类推！

另外，我们也可以看一看所有基因的表达概况：

``` bash
summary (raw-count_filt2)
```

> control1           control2             rep1               rep2
 Min.   :     0.0   Min.   :     0.0   Min.   :     0.0   Min.   :     0.0
 1st Qu.:     0.0   1st Qu.:     0.0   1st Qu.:     0.0   1st Qu.:     0.0
 Median :     0.0   Median :     0.0   Median :     1.0   Median :     1.0
 Mean   :   237.6   Mean   :   290.6   Mean   :   417.4   Mean   :   342.2
 3rd Qu.:    62.0   3rd Qu.:    65.0   3rd Qu.:    93.0   3rd Qu.:    71.0
 Max.   :108056.0   Max.   :164091.0   Max.   :171319.0   Max.   :132792.0

如果想要把分析结果保存下来，则存储为.csv格式：

``` bash
write.csv (x=raw_count_filt2, file="SRR35899.csv") # 保存raw_count_filt2矩阵为SRR35899.csv文件，保存位置在当前命令行目录中
```

