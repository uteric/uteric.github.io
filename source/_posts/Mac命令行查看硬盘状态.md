---
title: Mac命令行查看硬盘状态
date: 2018-03-22 22:29:47
updated: 2018-03-22 22:29:47
tags: 命令行
categories: 编程语言
---

## 查看整个硬盘 ##

``` bash
df -h # 查看整个硬盘的空间，h表明是人类可读的方式
```
> Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk1     233Gi  200Gi   33Gi    87% 52460543  8524799   86%   /
devfs          188Ki  188Ki    0Bi   100%      649        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home
/dev/disk2s1   931Gi  862Gi   70Gi    93%    97656 73042268    0%   /Volumes/TOSHIBA EXT

## 查看当前目录 ##

``` bash
du -d 1 -h # 查看当前目录下所有文件占用的硬盘空间
```
> 2.9G	./mothur #当前目录下有个mothur的文件夹，占用2.9GB
3.8G	. # 当前目录下其他文件占用3.8GB