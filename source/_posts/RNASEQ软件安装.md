---
title: RNASEQ分析入门笔记1-软件安装
date: 2018-03-21 23:39:36
updated: 2018-03-21 23:39:36
tags: RNASEQ
categories: 编程语言
---

RNASEQ的基本软件包括：*sra-tools, fastqc, hisat2, samtools, htseq, r, rstudio*，建议使用conda命令安装软件，降低出错概率，*conda is a tool for managing and deploying applications, environments and packages.*


## miniconda3 ##

在官网下载相应的版本，安装即可，https://conda.io/miniconda.html

安装完成后，需要创建频道，命令如下：

``` bash
conda config --add channels conda-forge
conda config --add channels defaults
conda config --add channels r
conda config --add channels bioconda
```
如果使用iTerm2作为终端，conda可能无法执行：*command not found: conda*

原因：终端是 iTerm2，安装了 zsh 和 oh-my-zsh，因此，打开终端时不再执行~/.bash_profile。

若使该文件有效，需要修改 zsh 的配置文件，方法如下：

编辑~/.zshrc，在文件末尾加入一行：

> source ~/.bash_profile

**注意：** .zshrc是一个位于根目录下的隐藏文件，如果要显示隐藏文件，请在终端中输入：

defaults write com.apple.finder AppleShowAllFiles -bool true;
KillAll Finder

## sra-tools ##

``` bash
conda install -c bioconda sra-tools=2.8.1
```

## fastqc ##

``` bash
conda install fastqc
```

## hisat2 ##

``` bash
conda install hisat2
```

## samtools ##

``` bash
conda install samtools
```

## htseq ##

``` bash
conda install -c bioconda htseq
```

## R ##

``` bash
conda install r
```

## Rstudio ##

``` bash
conda install rstudio
```

## wget ##

另外，下载NCBI上的原始数据（SRA文件），还需要用到wget，安装语法为：

``` bash
conda install wget 
```
