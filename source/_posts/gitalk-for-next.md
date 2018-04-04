---
title: 为NexT主题部署Gitalk评论插件
comments: true
date: 2017-12-04 09:29:58
udpated: 2018-01-13 11:30:31
categories:
 - Technology
 - Hexo
tags:
 - Gitalk
 - Gitment
 - NexT
---

在之前的文章中（{% post_link gitalk-comment-extension Gitalk评论插件 %}），已介绍过Gitalk评论插件，GitHub上为NexT添加Gitalk的pull request一直没有合入，自己手动本地部署吧。

## 为什么选择Gitalk

由于国内网络大环境的原因，Disqus 等优秀的评论系统都需要梯子才能使用，多说已经停运，NexT 已支持的 Gitment 目前存在一些问题，作者好像也已经很久没有维护了，故选择现在还在活跃的 Gitalk。

<!--more -->

## 新增插件代码

新建 layout/_third-party/comments/gitalk.swig 文件，添加以下代码：
``` js
{% if theme.gitalk.enable %}
  {% if page.comments %}
    <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
    <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
    <script type="text/javascript">
      const gitalk = new Gitalk({
        clientID: '{{theme.gitalk.clientID}}',
        clientSecret: '{{theme.gitalk.clientSecret}}',
        repo: '{{theme.gitalk.repo}}',
        owner: '{{theme.gitalk.owner}}',
        admin: '{{theme.gitalk.admin}}'.split(','),
        id: location.pathname,
        // facebook-like distraction free mode
        distractionFreeMode: '{{ theme.gitalk.distractionFreeMode }}',
        createIssueManually: '{{ theme.gitalk.createIssueManually }}'
      })
      gitalk.render('gitalk-container')
    </script>
  {% endif %}
{% endif %}
```

## 添加插件到NexT主题中

修改 layout/_third-party/comments/index.swig 文件，添加以下代码：
``` js
{% include 'gitalk.swig' %}
```

## 根据添加启用插件

修改 layout/_partials/comments.swig 文件，添加以下代码：
``` js
{% elseif theme.gitalk.enable %}
    <div class="comments" id="comments">
      <div id="gitalk-container"></div>
    </div>
```

## 在主题中增加Gitalk配置

修改 _config.yml 文件，增加以下代码：
``` js
# Gitalk
# more info please open https://github.com/gitalk/gitalk
gitalk:
  enable: true
  clientID: # Github Application Client ID
  clientSecret: # Github Application Client Secret
  repo: # Github repo
  owner: # Github repo owner
  admin: # support multiple admins split with comma, e.g. foo,bar
  distractionFreeMode: true # Facebook-like distraction free mode
  createIssueManually: true
```
填写相应的字段，通过 hexo 部署你的博客，即可看到评论插件效果。

