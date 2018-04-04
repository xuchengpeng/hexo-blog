---
title: Travis CI 构建时自动更新 NexT 主题源码
date: 2018-02-02 17:01:59
udpated: 2018-02-02 17:01:59
comments: true
categories:
 - Technology
 - Automation
tags:
 - Travis CI
 - NexT
---

在 {% post_link autodeploy-hexo-blog-with-travisci 使用 Travis CI 自动部署 Hexo 博客 %} 后，每次新增文章或者修改文章就会触发自动构建。

加入使用的主题代码更新，还需要手动合并后提交，如我的博客使用的是 NexT 主题，每天都有代码更新，所以还是想办法在构建的时候自动更新主题代码。

初步想法是在下载的博客代码后，生成博客之前，再下载一次博客的最新代码即可，Travis CI 构建过程有很多阶段，选择其中一个阶段即可。

1. OPTIONAL Install `apt addons`
2. OPTIONAL Install `cache components`
3. `before_install`
4. `install`
5. `before_script`
6. `script`
7. OPTIONAL `before_cache` (for cleaning up cache)
8. `after_success` or `after_failure`
9. OPTIONAL `before_deploy`
10. OPTIONAL `deploy`
11. OPTIONAL `after_deploy`
12. `after_script`

<!-- more -->

## 在 before_install 阶段更新 NexT 主题源码

在 `before_install` 阶段执行脚本 `update-theme-next.sh`，来完成主题源码更新：
```yml
before_install:
  - export TZ='Asia/Shanghai'
  - chmod +x ./update-theme-next.sh # 必须先给权限
  - ./update-theme-next.sh # 更新NexT主题
```

## 脚本更新 NexT 主题源码

```sh
#!/bin/bash
set -ev

rm -rf ./themes

# clone theme NexT
git clone https://github.com/theme-next/hexo-theme-next ./themes/next

# clone plugins
git clone https://github.com/theme-next/theme-next-needmoreshare2 ./themes/next/source/lib/needsharebutton
git clone https://github.com/theme-next/theme-next-fancybox3 ./themes/next/source/lib/fancybox
git clone https://github.com/theme-next/theme-next-pace ./themes/next/source/lib/pace
```

提交代码后可以看到 Travis CI 在构建时操作，log 如下：
```sh
$ export TZ='Asia/Shanghai'
$ chmod +x ./update-theme-next.sh
$ ./update-theme-next.sh
rm -rf ./themes
# clone theme NexT
git clone https://github.com/theme-next/hexo-theme-next ./themes/next
Cloning into './themes/next'...
# clone plugins
git clone https://github.com/theme-next/theme-next-needmoreshare2 ./themes/next/source/lib/needsharebutton
Cloning into './themes/next/source/lib/needsharebutton'...
git clone https://github.com/theme-next/theme-next-fancybox3 ./themes/next/source/lib/fancybox
Cloning into './themes/next/source/lib/fancybox'...
git clone https://github.com/theme-next/theme-next-pace ./themes/next/source/lib/pace
Cloning into './themes/next/source/lib/pace'...
```
