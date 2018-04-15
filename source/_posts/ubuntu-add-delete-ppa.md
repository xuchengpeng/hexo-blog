---
title: "Ubuntu 增加和删除 PPA 软件源"
comments: true
date: 2018-04-13 10:50:50
udpated: 2018-04-15 21:22:50
categories:
 - Technology
 - Linux
tags:
 - Ubuntu
 - PPA
 - emacs
---

PPA，英文全称为 Personal Package Archives，即个人软件包档案。是 Ubuntu Launchpad 网站提供的一项源服务，允许个人用户上传软件源代码，通过 Launchpad 进行编译并发布为二进制软件包，作为 apt / 新立得（Synaptic）源供其他用户下载和更新。

PPA 的一般形式是： **ppa:user/ppa-name**

添加 PPA 源:

* 添加 PPA 源的命令为： sudo add-apt-repository ppa:user/ppa-name
* 添加好记得要更新一下： sudo apt-get update

删除 PPA 源:

* 删除 PPA 源的命令格式则为：sudo add-apt-repository -r ppa:user/ppa-name
* 然后进入 /etc/apt/sources.list.d 目录，将相应 ppa 源的保存文件删除。
* 最后同样更新一下：sudo apt-get update
<!-- more -->

Ubuntu 下安装 emacs25：
```sh
$ sudo add-apt-repository ppa:kelleyk/emacs
# 或者是 sudo add-apt-repository ppa:ubuntu-elisp/ppa
$ sudo apt-get update
$ sudo apt-get install emacs25
# 或者是 sudo apt-get install emacs-snapshot
```
