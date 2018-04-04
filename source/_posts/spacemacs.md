---
title: spacemacs - Emacs advanced Kit focused on Evil
comments: true
date: 2017-11-28 11:06:50
udpate: 2018-01-14 10:45:50
categories:
  - Software
tags:
  - emacs
  - spacemacs
  - vim
---

![](/images/spacemacs-logo.svg)

A community-driven Emacs distribution - The best editor is neither Emacs nor Vim, it is Emacs and Vim!

这是一款定制化程度非常高的emacs配置，同时支持emacs和vim按键模式，无缝切换使用。

<!-- more-->

## 设定环境变量

如果是在Windows环境下，需要设定HOME环境变量的值，一般为 C:\Users\username。

## 安装字体

（可选）下载安装 [Source Code Pro](https://github.com/adobe-fonts/source-code-pro) 字体。

## 下载spacemacs

* 在~目录下（即HOME）创建文件夹.emacs.d；
* 从 [spacemacs](https://github.com/syl20bnr/spacemacs) 克隆develop分支的代码到.emacs.d文件夹下；
* 在.emacs.d文件夹下新建init.el文件，加入以下代码；
  ``` lisp
  (package-initialize)

  (setq spacemacs-start-directory "~/.emacs.d/spacemacs/")
  (load-file (concat spacemacs-start-directory "init.el"))
  ```

## 启动emacs

* 下载安装 [emacs](https://www.gnu.org/software/emacs/)；
* 根据提示选择适合自己的配置，完成后spacemacs的配置保存在 ~/.spacemacs 中 {% button https://github.com/xuchengpeng/myspacemacs/raw/master/.spacemacs, 我的配置文件, download fa-lg fa-fw %}；
* 如果启动中遇到以下提示：
  {% note info%}
  The directory ~/.emacs.d/server is unsafe
  {% endnote %}
  在server文件夹 “属性-安全-高级-所有者” 中修改所属用户。

