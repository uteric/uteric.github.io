---
title: Ubuntu 18.04上为无线网卡RTL8821CE安装驱动
date: 2019-12-25 11:21:15
updated: 2019-12-25 11:21:15
tags: LINUX
categories: 编程语言
---


今年黑色星期五在网上买了一台联想IdeaPad S145，买回来后，打算安装**Ubuntu 18.04**，万万没想到的是，安装系统后发现找不到无线网卡，使用命令*sudo lshw -C network*杳看网络信息，发现这台电脑的网卡是RTL8821CE，然而，目前LINUX还没有相应的官方驱动，因此我这个小白花了大把时间来安装网卡驱动，过程可以说是非常艰苦，花了一个星期时间，记录下来或许能帮到后来人。

但是，如果你的电脑能够联网，整个过程将会非常简单，首先，需要在GITHUB下载并安装[RTL8821CE](https://github.com/tomaspinho/rtl8821ce)驱动。

安装驱动需要用到*dkms*和*git*，在联网的情况下，命令如下：

```
sudo apt update
sudo apt install -y dkms git
```

*dkms*和*git*成功安装后，下载并安装驱动程序即可，命令如下：

```
git clone https://github.com/tomaspinho/rtl8821ce
cd rtl8821ce
sudo ./dkms-install.sh
```

然而，事情远没那么简单，成功运行前面代码的前提是联网，但我的电脑不能联网。最神奇的是，这台便宜的电脑居然连网线接口都没有，想接有线网都不可能，在这种情况下，安装*dkms*就难得异乎寻常，因为*dkms*本身有一堆Dependencies，而每个Dependency又有一堆Dependencies，甚至不同的包之间还彼此互相依赖。

在这种情况下，我不得不采用迂回的方式来安装*dkms*。首先，在WINDOWS下安装虚拟机VM，运行Unbuntu 18.04，通过以下命令来获取安装*dkms*所需的包以及驱动程序：

```
sudo apt install -y git apt-rdepends
git clone https://github.com/tomaspinho/rtl8821ce
mkdir debs
cd debs
apt-get download $(apt-rdepends dkms | grep -v "^ ")
```
在运行apt-rdepends过程中，如果lib-dev无法下载，将命令改成如下将其忽略：

```
apt-get download $(apt-rdepends dkms | grep -v "^ " | grep -v "^lib-dev")
```

将下载到的RTL8821CE和debs文件夹通过U盘拷贝到Unbuntu系统中，下面就可以真正开始安装网卡驱动了，命令如下：

1）Disable Machine Owner Key，执行代码后需要重启，会出现蓝屏，按照引导操作即可

```
sudo mokutil --disable-validation
```

2）切换到安装包所在目录，安装dkms及其依赖包

```
cd debs
sudo dpkg -i * 

```

3）进入驱动目录，安装驱动

```
make
sudo make install
sudo ./dkms-install.sh
```

安装完成后，重启即可看到网卡驱动。在此，特别向参考文献中的两位大神致敬。

## 参考文献

[1] [Install Realtek RTL8821CE Wifi Driver on Ubuntu 18.04](https://xianwen.me/2019/09/04/install-realtek-rtl8821ce-wifi-driver-on-ubuntu-18-04/)

[2] [How to install Wi-Fi driver for Realtek RTL8821CE on Ubuntu 18.04? ](https://askubuntu.com/questions/1071299/how-to-install-wi-fi-driver-for-realtek-rtl8821ce-on-ubuntu-18-04)
