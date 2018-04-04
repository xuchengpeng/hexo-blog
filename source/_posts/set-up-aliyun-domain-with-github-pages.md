---
title: GitHub Pages 绑定阿里云域名
comments: true
date: 2018-01-16 11:09:48
udpated: 2018-02-08 17:45:48
categories:
 - Technology
 - Web
tags:
 - Aliyun
 - GitHub Pages
---

![](/images/aliyun_logo.png)

首先在 [阿里云](https://www.aliyun.com) 上注册一个域名，阿里云域名更有 [域名一元购专题](https://wanwang.aliyun.com/domain/1yuan)，价格十分便宜。

<!-- more -->

## 设置Custom domain

在 GitHub 博客工程中进行设置，{% label info@Settings - Options %} 中找到 {% label info@Custom domain %} 设定申请的域名，例如我申请的是 `xuchengpeng.com`。
设置 Custom Domain 会自动在根目录创建 CNAME 文件。

## 创建CNAME文件

在 GitHub 博客工程根目录创建 CNAME 文件，内容填上申请的域名 `xuchengpeng.com`。
如果是 Hexo 博客，则在 `source` 目录下创建 CNAME 文件，填好内容后发布即可。

## 绑定域名

在阿里云域名解析设置中增加如下配置：

| 记录类型       | 主机记录       | 解析线路       | 记录值         | TTL值          |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| CNAME          | www            | 默认           | xuchengpeng.github.io | 600            |
| A              | @              | 默认           | 192.30.252.153 | 600            |
| A              | @              | 默认           | 192.30.252.154 | 600            |

* `xuchengpeng.github.io` 是 GitHub Pages 博客的地址；
* `192.30.252.153` 和 `192.30.252.154` 是 GitHub Pages 服务器指定的IP地址，在 [这里](https://help.github.com/articles/setting-up-an-apex-domain/) 可以找到。

设置完成之后，等待一会儿域名就生效了，可以通过 http://xuchengpeng.com/ 访问博客。
