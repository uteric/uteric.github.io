---
title: MAC命令行操作MacVim
date: 2018-03-27 22:50:22
updated: 2018-03-27 22:50:22
tags: MAC
categories: 编程语言
---

**1. 打开MacVim**
``` bash
vim ./test.md # 打开当前目录下的名位test.md的文件
```

**2. 在Vim中修改文件**
在Vim中打开的文件不能直接修改，如要插入字符，需要按下i键，进入INSERT模式，按下ESC则可退出INSERT模式。

**3. 退出Vim**

*在末行模式下：*
``` bash
:qw # 先保存文档，退出Vim，返回到Shell
:q! #放弃修改，直接返回到Shell
```

*在命令行模式下：*
``` bash
ZZ # 保存修改，退出Vim，返回到Shell
```


