---
title:  "Gitment评论系统"
date: 2017-11-22 16:00:00
updated: 2017-12-14 08:50:00
comments: true
categories: 
  - Technology
tags:
  - GitHub
  - Gitment
---

[Gitment](https://github.com/imsun/gitment) 是作者实现的一款基于 GitHub Issues 的评论系统。支持在前端直接引入，不需要任何后端代码。可以在页面进行登录、查看、评论、点赞等操作，同时有完整的 Markdown / GFM 和代码高亮支持。尤为适合各种基于 GitHub Pages 的静态博客或项目页面。

<!-- more -->

## 注册 OAuth Application

[点击此处](https://github.com/settings/applications/new) 来注册一个新的 OAuth Application。其他内容可以随意填写，但要确保填入正确的 callback URL（一般是博客评论页面对应的域名，例如 https://xxxxxx.github.io ）。

你会得到一个 client ID 和一个 client secret，这个将被用于之后的用户登录。

## 引入 Gitment

将下面的代码添加到你的页面：

```html
<div id="container"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
var gitment = new Gitment({
  id: '页面 ID', // 可选。默认为 location.href
  owner: '你的 GitHub ID',
  repo: '存储评论的 repo，可以自己新建一个，也可以直接使用博客的 repo',
  oauth: {
    client_id: '你的 client ID',
    client_secret: '你的 client secret',
  },
})
gitment.render('container')
</script>
```

注意，上述代码引用的 Gitment 将会随着开发变动。如果你希望始终使用最新的界面与特性即可引入上述代码。

## 初始化评论

在博客页面最下面，登陆 GitHub 的帐号，点击初始化按钮，之后其他用户即可在该页面发表评论。
