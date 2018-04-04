---
title: 通过 Cloudflare 给绑定个人域名的GitHub Pages博客加持https
date: 2018-01-29 07:58:19
udpated: 2018-02-08 17:43:19
comments: true
categories:
 - Technology
 - Web
tags:
 - https
 - Cloudflare
---

![](/images/https&http.jpg)

> Google宣布了，从2017年1月份正式发布的Chrome 56开始，Google将把某些包含敏感内容的https页面标记为“不安全”。

GitHub Pages 本身是 https 的站点，但是 {% post_link set-up-aliyun-domain-with-github-pages 绑定个人域名 %} 了以后，站点便是 http 的。

## https 和 http 的区别

1. https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
2. http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

<!-- more -->

## https 的优点

尽管HTTPS并非绝对安全，掌握根证书的机构、掌握加密算法的组织同样可以进行中间人形式的攻击，但HTTPS仍是现行架构下最安全的解决方案，主要有以下几个好处：
1. 使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；
2. HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。
3. HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。
4. 谷歌曾在2014年8月份调整搜索引擎算法，并称“比起同等HTTP网站，采用HTTPS加密的网站在搜索结果中的排名将会更高”。

## https 的缺点

虽然说HTTPS有很大的优势，但其相对来说，还是存在不足之处的：
1. HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；
2. HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；
3. SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。
4. SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。
5. HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

## 为什么使用 Cloudflare 提供的免费SSL

收费的SSL服务总是比免费的更加周到，一般收费的SSL都会提供端到端的加密。但是价格不菲，对于个人博客来说，使用免费的就可以了。Cloudflare还提供免费的CDN和缓存技术，让浏览者有更好的体验。

## 使用 Cloudflare 管理站点

1、注册 [Cloudflare](https://www.cloudflare.com/) 账号并登陆。
2、添加你的站点，填入域名，比如我的是 `xuchengpeng.com`，点击 **Begin Scan**。
3、Cloudflare 会花大约60秒的时间扫描 DNS 记录，确保有以下几条：

| 记录类型       | 主机记录       | 解析线路       | 记录值         | TTL值          |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| CNAME          | www            | 默认           | xuchengpeng.github.io | 600            |
| A              | @              | 默认           | 192.30.252.153 | 600            |
| A              | @              | 默认           | 192.30.252.154 | 600            |

4、继续点击 **Continue**，到域名注册商网站把 DNS 修改为 Cloudflare 提供的 DNS 服务器地址。
   如我的是：`aiden.ns.cloudflare.com` 和 `kate.ns.cloudflare.com`。
5、选择 **Free Website**，直至建站完成。
6、点击 **Crypto** 菜单，设置为 **Flexible**。
打开 **Always use HTTPS** 和 **Automatic HTTPS Rewrites** 选项。
7、点击 **Page Rules** 菜单，增加两条规则：

| 地址           | 规则           |
| :------------- | :------------- |
| http://xuchengpeng.com/* | Always Use HTTPS            |
| http://xuchengpeng.com/ | Forwarding URL:(Status Code: 301 - Permanent Redirect, Url: https://xuchengpeng.com/) |

8、等待一段时间，设置生效，访问你的网站即可看到效果。
