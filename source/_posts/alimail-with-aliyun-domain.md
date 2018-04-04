---
title: 阿里云域名赠送企业邮箱
date: 2018-01-18 07:30:32
udpated: 2018-02-08 17:44:32
comments: true
categories:
 - Technology
 - Web
tags:
 - Aliyun
 - E-mail
---

![](/images/alimail-qiye.jpg)

申请 {% post_link set-up-aliyun-domain-with-github-pages 阿里云域名 %} 后，会赠送阿里云企业邮箱免费版。
* 50个帐号，5G/帐号；
* 域名注册成功后，自动开通企业邮箱并添加邮箱解析；
* 每位会员只能开通一个企业邮箱免费版;
* 仅 .top 域名注册送免费企业邮箱。

开通企业邮箱免费版后一定要尽快使其生效，否则七天内会被收回。

<!-- more -->

## 域名绑定

进入 [阿里云邮箱控制台](https://alimail.console.aliyun.com) 即可看到拥有的企业邮箱免费版。
在更换域名选项卡中，选择你申请的域名并提交。

## 设置解析

一般情况下，域名注册后会自动添加邮箱解析，解析状态会显示 {% label success@解析已生效 %}；
如果邮箱解析失败，可以尝试一键添加解析，也可以手动添加如下配置：

| 记录类型       | 主机记录       | 解析线路       | 记录值         | MX优先级       | TTL值          |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| TXT            | @              | 默认           | v=spf1 include:spf.mxhichina.com -all |        | 600            |
| CNAME          | mail           | 默认           | mail.mxhichina.com |            | 600            |
| CNAME          | imap           | 默认           | imap.mxhichina.com |            | 600            |
| CNAME          | smtp           | 默认           | smtp.mxhichina.com |            | 600            |
| CNAME          | pop3           | 默认           | pop3.mxhichina.com |            | 600            |
| MX             | @              | 默认           | mxw.mxhichina.com | 10          | 600            |
| MX             | @              | 默认           | mxn.mxhichina.com | 5           | 600            |

添加完成后刷新域名解析即可看到解析状态中显示 {% label success@解析已生效 %}。

## 设置密码和安全问题

在 {% label info@重置密码 %} 和 {% label info@重置安全问题 %} 选项卡中设置管理员帐号的密码和安全问题。

## 分配帐号

使用管理员帐号登陆 [邮箱访问地址](https://qiye.aliyun.com) 为用户分配帐号。
