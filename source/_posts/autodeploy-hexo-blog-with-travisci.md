---
title: 使用 Travis CI 自动部署 Hexo 博客
date: 2018-01-30 09:30:40
udpated: 2018-02-08 17:42:40
comments: true
categories:
 - Technology
 - Automation
tags:
 - Travis CI
 - Hexo
---

![](/images/travis-ci.jpg)

CI 是 `Continuous Integration` 的缩写，持续集成之意。

> 持续集成是一种软件开发实践，每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。

[Travis CI](https://travis-ci.org/) 是目前新兴的开源持续集成构建项目，用来构建托管在GitHub上的代码。它提供了多种编程语言的支持，包括Ruby，JavaScript，Java，Scala，PHP，Haskell和Erlang在内的多种语言。许多知名的开源项目使用它来在每次提交的时候进行构建测试，比如Ruby on Rails，Ruby和Node.js。

> Travis CI是在软件开发领域中的一个在线的，分布式的持续集成服务，用来构建及测试在GitHub托管的代码。这个软件的代码同时也是开源的，可以在GitHub上下载到，尽管开发者当前并不推荐在闭源项目中单独使用它。

## 工作原理

当我们每次进行push等动作时，Travis CI 会自动检测我们的提交，然后根据配置文件，搭建虚拟主机来运行测试，构建等指令。在这里，就是运行 `hexo deploy --generate` 等命令来自动生成、部署静态网页。

<!-- more -->

## Hexo 搭建

在 GitHub 上新建 `xuchengpeng.github.io` 工程，其中 `master` 分支用来部署博客静态文件，`hexo` 分支用来存储源码。

hexo 分支只需要提交以下文件即可：
```
.
├── scaffolds
├── source
├── themes
├── .gitignore
├── .travis.yml
├── _config.yml
└── package.json
```

## 配置 Travis CI

使用 GitHub 帐号登陆 Travis CI，会自动关联 GitHub 上面的仓库，开启 `xuchengpeng/xuchengpeng.github.io` 仓库。

进入设置选项，勾选 `Build only if .travis.yml is preset`，`Build pushed branches` 和 `Build pushed pull requests`。

## 配置 Access Tokens

登陆 [GitHub](https://github.com/)，在 `Settings / Developer settings / Person access tokens` 下，点击 **Generate new token** 新建一个 **TRAVIS_TOKEN** 并且复制生成的值字符串，后面需要使用，权限只需要勾选 `repo` 即可。

在 Travis CI 博客项目设置下配置 `Environment Variables`，`Name` 填写 `TRAVIS_TOKEN`，`Value` 填写刚才保存的 GitHub 生成的值字符串，不要勾选 `Display value in build log` 选项，点击 **Add** 按钮增加配置。

此时，Travis CI 就已经获取了 GitHub 仓库的操作权限。

## 配置 .travis.yml

```yml
language: node_js  #设置语言

node_js: stable  #设置相应的版本

notifications:
  email: # 构建完成后邮件通知
    recipients:
      - 330476629@qq.com
      - xucp@outlook.com
    on_success: always # default: change
    on_failure: always # default: always

install:
  - npm install  #安装hexo及插件

script:
  - hexo clean  #清除
  - hexo generate  #生成

after_script:
  - cd ./public
  - git init
  - git config user.name "xuchengpeng"  #修改name
  - git config user.email "330476629@qq.com"  #修改email
  - git add .
  - git commit -m "Site updated"
  - git push --force --quiet "https://${TRAVIS_TOKEN}@${GH_REF}" master:master  #TRAVIS_TOKEN是在Travis中配置token的名称

branches:
  only:
    - hexo  #只监测hexo分支，hexo是我的分支的名称，可根据自己情况设置

env:
 global:
   - GH_REF: github.com/xuchengpeng/xuchengpeng.github.io.git # GitHub博客仓库的地址
```

## 自动部署

当 `.travis.yml` 配置文件修改完成后，将其提交到 GitHub 仓库的 `hexo` 分支下，在 Travis CI 博客项目下就可以看到自动构建开始执行，在 `Job log` 可以看到构建和部署的整个过程。

以后只要有新的修改提交到 `hexo` 分支，Travis CI 都会按照上述过程自动构建和部署，打开博客主页即可看到部署的效果。

## Travis CI 构建状态

在 GitHub 博客仓库 `README.md` 文件下增加以下代码，即可看到 Travis CI 构建状态。
```markdown
[![Build Status](https://travis-ci.org/xuchengpeng/xuchengpeng.github.io.svg?branch=hexo)](https://travis-ci.org/xuchengpeng/xuchengpeng.github.io)
```
