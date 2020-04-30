---
title: MAC安装DESeq2报错及解决方案
date: 2018-04-04 20:43:45
updated: 2018-04-04 20:43:45
tags: MAC
categories: 编程语言
---

## 在R环境中安装DESeq2 ##

**1、安装DESeq2基本语法：**

``` bash
source ("http://bioconductor.org/biocLite.R") # 调入安装工具Bioconductor
biocLite("DESeq2") # 安装DESeq2
library("DESeq2") # 测试是否安装成功
```

**2、报错信息**

我的系统是MacOS 10.10.5，执行安装命令后各种报错：

---
~~fatal error:
**'string.h' file not found**
include <string.h>            /* for memcpy, memset */
1 error generated.
make: *** [000.init.o] Error 1
ERROR: compilation failed for package ‘matrixStats’~~

---
~~fatal error: 
**'math.h' file not found**
include_next <math.h>
1 error generated.
make: *** [DbColumn.o] Error 1
ERROR: compilation failed for package ‘RSQLite’~~

---
~~fatal error: 
**'ctype.h' file not found**
include <ctype.h>
1 error generated.
make: *** [DocParse.o] Error 1
ERROR: compilation failed for package ‘XML’~~

---
~~fatal error: 
**'stdlib.h' file not found**
include <stdlib.h> /* Not used by R itself, but widely assumed in packages */
1 error generated.
make: *** [all_missing.o] Error 1
ERROR: compilation failed for package ‘checkmate’~~

---
~~fatal error: 
**'string.h' file not found**
include <string.h>            
/* for memcpy, memset */
1 error generated.
make: *** [AEbufs.o] Error 1
ERROR: compilation failed for package ‘S4Vectors’~~

---
~~fatal error:
**'stdio.h' file not found**
include <stdio.h>
1 error generated.
make: *** [Rinit.o] Error 1
ERROR: compilation failed for package ‘Biobase’~~

---
~~fatal error: 
**'unistd.h' file not found**
include <unistd.h>
1 error generated.
make: *** [ipcmutex.o] Error 1
ERROR: compilation failed for package ‘BiocParallel’~~

---
~~fatal error: 
**'stdlib.h' file not found**
include <stdlib.h>
1 error generated.
make: *** [S_enter.o] Error 1
ERROR: compilation failed for package ‘locfit’~~

---
~~configure: error: 
**cannot run C++ compiled programs.**
If you meant to cross compile, use `--host'.
See `config.log' for more details
ERROR: configuration failed for package ‘RcppArmadillo’~~

---

*There were 22 warnings (use warnings() to see them)*
**warnings()**
Warning messages:
1: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘matrixStats’ had non-zero exit status
2: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘RSQLite’ had non-zero exit status
3: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘XML’ had non-zero exit status
4: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘checkmate’ had non-zero exit status
5: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘S4Vectors’ had non-zero exit status
6: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘Biobase’ had non-zero exit status
7: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘BiocParallel’ had non-zero exit status
8: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘locfit’ had non-zero exit status
9: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘RcppArmadillo’ had non-zero exit status
10: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘htmlTable’ had non-zero exit status
11: In install.packages(pkgs = doing, lib = lib, ...) :
 installation of package ‘IRanges’ had non-zero exit status
12: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘GenomeInfoDb’ had non-zero exit status
13: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘XVector’ had non-zero exit status
14: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘DelayedArray’ had non-zero exit status
15: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘AnnotationDbi’ had non-zero exit status
16: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘Hmisc’ had non-zero exit status
17: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘annotate’ had non-zero exit status
18: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘GenomicRanges’ had non-zero exit status
19: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘SummarizedExperiment’ had non-zero exit status
20: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘genefilter’ had non-zero exit status
21: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘geneplotter’ had non-zero exit status
22: In install.packages(pkgs = doing, lib = lib, ...) :
installation of package ‘DESeq2’ had non-zero exit status

---

**3、报错原因**

根据返回的报错信息，可以知道，系统中缺乏'string.h'，'math.h'，'ctype.h'，'stdlib.h'，'stdio.h'，'unistd.h'这些文件，这些文件都是C++ library中得Header files，因此，需要安装C++，或者直接安装Xcode Command Line Tools。

---

## 安装Xcode命令行工具 ##

Xcode Command Line Tools，包含一些命令行开发工具，例如gcc/g++编译器、make、git、nasm等，安装命令如下：

``` bash
xcode-select --install # 安装Xcode命令行工具（Xcode Command Line Tools）
```

安装完成后，检查是否成功：

``` bash
xcode-select -p # 检查是否成功安装
```
> /Applications/Xcode.app/Contents/Developer

若看到如下信息，则表明Xcode Command Line Tools已经安装：

> xcode-select: error: command line tools are already installed, use "Software Update" to install update

---

## 再次安装DESeq2 ##

此时，就可以着手安装DESeq2软件包了，执行：

``` bash
source ("http://bioconductor.org/biocLite.R") # 调入安装工具Bioconductor
biocLite("DESeq2") # 安装DESeq2
```



