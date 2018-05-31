---
title: "Ubuntu 升级内核"
comments: true
date: 2018-04-20 15:34:34
udpated: 2018-05-31 16:54:34
categories:
 - Technology
 - Linux
tags:
 - Ubuntu
 - Linode
---

### 查看当前内核版本
```sh
$ uname -r
4.4.0-53-generic
```

### 下载内核
下载最新 Ubuntu 编译好的内核地址： http://kernel.ubuntu.com/~kernel-ppa/mainline/
选取当前最新的内核 4.16.3 版本进行下载：
```sh
$ mkdir 4.16.3 && cd 4.16.3
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.16.3/linux-headers-4.16.3-041603_4.16.3-041603.201804190730_all.deb
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.16.3/linux-headers-4.16.3-041603-generic_4.16.3-041603.201804190730_amd64.deb
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.16.3/linux-image-4.16.3-041603-generic_4.16.3-041603.201804190730_amd64.deb
```

### 安装内核
```sh
$ dpkg -i *.deb
```

### 卸载旧内核
```sh
# 查看目前安装的内核
$ dpkg -l | grep linux
ii  linux-image-4.4.0-53-generic       4.4.0-53.74                        amd64        Linux kernel image for version 4.4.0 on 64 bit x86 SMP
ii  linux-image-4.16.3-041603-generic  4.16.3-041603.201804190730         amd64        Linux kernel image for version 4.16.3 on 64 bit x86 SMP
## 卸载旧内核
$ apt-get purge linux-image-4.4.0-53-generic linux-headers-4.4.0-53
```

如果查询的结果中还有显示已卸载的内核，可以使用`dpkg -P linux-image-4.4.0-53-generic`命令将其删除。

### 更新启动引导
```sh
$ update-grub
# 重启后再查看内核版本验证 uname -a
$ reboot
```
<!-- more -->

### Linode
在 Linode 提供的服务器上面，需要将 Boot Setting 下 Kernel 设定为 **GRUB 2**。
