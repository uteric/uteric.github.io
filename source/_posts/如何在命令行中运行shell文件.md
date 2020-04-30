---
title: 如何在命令行中运行shell文件
date: 2018-03-22 22:15:47
updated: 2018-03-22 22:15:47
tags: 命令行
categories: 编程语言
---

使用VIM新建文本一个.sh，比如srawget.sh

打开终端，进入当前目录，输入

``` bash
./srawget.sh #当前目录下执行srwaget.sh文件
```
执行后返回：

> zsh: permission denied: ./srawget.sh

发现.sh文件不能执行，为什么呢？

这是因为VIM新建的shell文件默认权限只有--r--w，即只能读写，不能执行，因此出现上述错误，解决办法其实非常简单，只要修改.sh文件的权限即可，在终端中输入：

``` bash
chmod +x srawget.sh #添加执行权限
```

我们使用 ls -l 命令来查看一下，发现已经成功添加了执行权限，再次执行此命令即可：

``` bash
./srawget.sh
```
