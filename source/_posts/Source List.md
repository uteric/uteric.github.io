---
title: 编辑sources.list
date: 2019-12-25 11:21:23
updated: 2019-12-25 11:21:23
tags: Ubuntu
categories: 编程语言
---


Ubuntu 的软件源配置文件是 /etc/apt/sources.list，进行修改之前建议将系统自带的该文件做个备份，执行如下命令：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```
打开并编辑

```
sudo gedit /etc/apt/sources.list
sudo apt-get update
```
