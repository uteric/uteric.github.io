---
title: Git基础命令
date: 2017-08-18 20:54:58
updated: 2017-08-18 20:54:58
tags:
  - Git
categories: 编程语言
---


## 判断Git是否安装成功

``` bash
git
```
## 创建目录

``` bash
mkdir <dirname>
```

mkdir是make directory的缩写，意为“创建目录”；比如，~~mkdir test~~命令将在当前目录下创建一个名为test的新目录；

## 切换目录

``` bash
cd <dirname>
```
cd是change directory的缩写，意为“切换目录”；比如，~~cd test~~命令将从当前目录切换到刚刚创建的test目录；

## 创建文件

``` bash
touch <filename>
```

touch命令的作用是change file access and modification times，即创建空文件或者修改文件的时间特性；比如，~~touch test.md~~命令可在当前目录中创建名为test.md的空白文件；

## 查看Git仓库状态

``` bash
git status
```
**git status**命令用于查看Git仓库的状态，如果当前目录还不是Git仓库，则会报错：~~fatal: Not a git repository (or any of the parent directories): .git~~; 这时，需要用到**git init**命令来初始化Git仓库；

## 初始化Git仓库

``` bash
git init
```

**git init**命令用于初始化Git仓库，将当前目录初始化为Git仓库；初始化Git仓库后，如果再次输入**git status**命令，则会提示~~on branch master, initial commit, untracked files: (use "git add <file>..." to include in what will be commited), mothing added to commit but untracked files present (use "git add" to track)~~，意思是说，新生成的Git仓库默认在主分支（master branch），而且，当前Git仓库中有未被跟踪（待提交）的文件，即刚刚使用**touch**命令新建的test.md文件，如果需要提交此文件，则可以用**git add**命令进行操作；

## 文件等待提交

``` bash
git add
```

刚刚谈到，新建的test.md尚未提交到Git仓库，如要提交，可以使用**git add test.md**命令；完成之后，我们再使用**git status**命令来检查一下Git仓库的状态，显示~~On branch master, initial commit, Changes to be commited: (use "git reset HEAD <file>..." to unstage)~~，意思是说，test.md文件已被写入缓存，等待提交，这个时候，你可以使用**git reset HEAD test.md**命令来移除这个缓存，回滚到未被跟踪的状态；

## 提交文件至Git仓库

``` bash
git commit -m
```

**git commit -m**命令用于将文件提交至Git仓库，比如，**git commit -m 'This is my first commit'**，执行后提示test.md文件已被创建，同时显示提交时的附带信息“This is my first commit"；此时，如果执行**git status**，则会显示~~nothing to commit, working directory clean~~;

## 查看提交记录

``` bash
git log
```

**git log**命令可以查看commit记录，包括作者，时间，以及提交时的附加信息，如”This is my first commit"; 如要退出git log，输入q即可；








