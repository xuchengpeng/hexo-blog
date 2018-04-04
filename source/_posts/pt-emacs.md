---
title: The Platinum Seacher for Emacs
comments: true
date: 2017-11-29 09:50:49
udpate: 2018-03-17 17:50:49
categories:
 - Software
 - Search
tags:
 - emacs
 - spacemacs
 - pt
---

[pt](https://github.com/monochromegane/the_platinum_searcher) is a code search tool similar to ack and the_silver_searcher(ag). It supports multi platforms and multi encodings.

![](/images/pt-search-demo.jpg)

<!--more -->

## 特点

* 它搜索代码的速度是 ack 的3-5倍；
* 它搜索代码的速度和 ag 一样快；
* 它忽略 .gitignore 中定义的文件；
* 它搜索 UTF-8，EUC-JP，Shift_JIS 文件；
* 提供多个操作系统平台的运行文件（macOS，Windows，Linux）；

## 速度

ack < ag < pt

## 安装

* **[点击这里](https://github.com/monochromegane/the_platinum_searcher/releases)** 下载所需的二进制执行文件；
* 将可执行文件的目录加入 PATH 环境变量；
* emacs 从 [MELPA](https://melpa.org/) 安装 pt.el；
* spacemacs 在 dotspacemacs-additional-packages 配置中增加 pt；

## 使用

* 命令行
  ``` sh
  $ # Recursively searches for PATTERN in current directory.
  $ pt PATTERN
  
  $ # You can specify PATH and some OPTIONS.
  $ pt OPTIONS PATTERN PATH
  ```
* spacemacs
  **M-x pt-regexp** or **M-x projectile-pt**



