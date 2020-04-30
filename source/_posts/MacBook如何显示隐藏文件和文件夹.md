---
title: MacBook如何显示隐藏文件和文件夹
date: 2017-08-18 20:32:08
updated: 2017-08-18 20:32:08
tags:
  - MacBook
  - Finder
  - 终端
categories: 编程语言
---

默认情况下，MacBook的系统配置文件是隐藏的，但是有些时候我们又需要对这些配置文件进行操作，这时，就需要将隐藏的文件和文件夹显示出来，方法如下：

## 第一步，打开终端「Terminal」；

## 第二步，输入如下命令，按「Return」键执行；

``` bash
defaults write com.apple.finder AppleShowAllFiles -boolean true
```

## 第三步，重启Finder，即可看到隐藏的文件和文件夹了；

如果需要再次隐藏文件和文件夹怎么办呢？只需要将上述命令中得布尔值改为false即可；

``` bash
defaults write com.apple.finder AppleShowAllFiles -boolean false
```

同样，重启Finder后，相应的文件和文件夹就被再次隐藏起来。