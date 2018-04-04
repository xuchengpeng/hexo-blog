---
title: "Gitalk评论插件"
date: 2017-11-24 14:51:01
updated: 2017-11-24 14:51:01
comments: true
categories:
 - Technology
tags:
 - Hexo
 - Gitalk
 - Gitment
---

[Gitalk](https://github.com/gitalk/gitalk) 是一个基于 Github Issue 和 Preact 开发的评论插件。

<!-- more -->

## 特性
* 使用 Github 登录
* 支持多语言 [en, zh-CN, zh-TW, es-ES, fr]
* 支持个人或组织
* 无干扰模式（设置 distractionFreeMode 为 true 开启）
* 快捷键提交评论 （cmd|ctrl + enter）

## 安装
* 直接引入
``` html
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```
* npm 安装
  ``` sh
  npm i --save gitalk
  ```
  ``` js
  import 'gitalk/dist/gitalk.css'
  import Gitalk from 'gitalk'
  ```

## 使用
需要 Github Application，如果没有 [点击这里申请](https://github.com/settings/applications/new)，Authorization callback URL 填写当前使用插件页面的域名。
``` js
const gitalk = new Gitalk({
  clientID: 'Github Application Client ID',
  clientSecret: 'Github Application Client Secret',
  repo: 'Github repo',
  owner: 'Github repo owner',
  admin: ['Github repo owner and collaborators, only these guys can initialize github issues'],
  // facebook-like distraction free mode
  distractionFreeMode: false
})

gitalk.render('gitalk-container')
```

## 类似项目

* {% post_link gitment-comment-system Gitment评论系统 %}