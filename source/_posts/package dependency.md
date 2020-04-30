---
title: 如何解决LINUX安装包相互依赖
date: 2019-12-25 11:14:54
updated: 2019-12-25 11:14:54
tags: LINUX
categories: 编程语言
---



命令如下：

```
sudo apt install -y git apt-rdepends
mkdir debs
cd debs
apt-get download $(apt-rdepends dkms | grep -v "^ ")
```
在运行apt-rdepends过程中，若某个包无法下载，提示no candicate，比如lib-dev无法下载，将命令改成如下将其忽略：

```
apt-get download $(apt-rdepends dkms | grep -v "^ " | grep -v "^lib-dev")
```
下载完成后，切换到debs文件夹，执行安装：

```
cd debs
sudo dpkg -i *
```

