---
title: MacBook设置zsh为默认Shell
date: 2017-08-17 23:16:00
updated: 2017-08-17 23:16:00
tags:
  - MacBook
  - bash
  - zsh
  - shell
categories: 编程语言
---

## 查看MacBook当前使用的Shell

``` bash
echo $SHELL
```

默认情况下，MacBook使用bash作为Shell，通过上面的命令即可查看，如返回结果为：/bin/bash，则说明MacBook正在使用bash作为默认Shell；

## 查看zsh的安装路径

``` bash
which zsh
```

MacBook自带zsh，通过which zsh命令可以查看zsh的安装路径，不同的操作系统版本可能不同，如返回结果为：/bin/zsh，则代表zsh的安装路径，记住这个路径，下面需要用到；

MacBook默认支持多种shells，因此，你也可以通过执行 more /etc/shells命令来查看当前系统支持哪些Shell，并列出所有Shell的安装路径；

``` bash
more /etc/shells
```

## 设置zsh为默认的shell

``` bash
chsh -s /bin/zsh
```

获得zsh的安装路径之后，即可执行如上命令，将默认的shell从bash切换为zsh，**注意**：请根据你的zsh路径替换/bin/zsh；

执行上述命令之后，系统回提示你输入密码，密码为暗码，屏幕上不可见，输入完后直接按Enter，重启Terminal，就从bash切换到zsh，**Cheers**!