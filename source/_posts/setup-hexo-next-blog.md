---
title: "搭建Hexo NexT主题博客"
comments: true
date: 2018-01-09 14:41:31
udpated: 2018-04-24 16:58:31
categories:
 - Technology
 - Hexo
tags:
 - Hexo
 - NexT
---

![](/images/NextSchemes3.png)

## 什么是Hexo？
[Hexo](https://hexo.io/) 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

[NexT](https://github.com/theme-next/hexo-theme-next) 是一款简洁优雅且易于使用的主题。

在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 {% label primary@站点配置文件 %}， 后者称为 {% label info@主题配置文件 %}。

<!-- more -->

## 安装

### 安装前提
安装 [Node.js](https://nodejs.org/) 和 [Git](https://git-scm.com/)。

### 安装 Hexo
```sh
npm install -g hexo-cli
```

## 建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```sh
hexo init <folder>
cd <folder>
npm install
```
新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## 配置
修改 {% label primary@站点配置文件 %}。

### 网站
| 参数           | 描述          |
| :------------- | :------------- |
| title          | xuchengpeng |
| subtitle       | Valar Morghulis. Valar Dohaeris. |
| description    | Valar Morghulis. Valar Dohaeris. |
| author         | xuchengpeng |
| language       | zh-CN |
| timezone       | Asia/Shanghai |

### 网址
| 参数           | 描述          |
| :------------- | :------------- |
| url            | http://xuchengpeng.com/ |
| permalink      | :year/:month/:day/:title/ |

## 安装 NexT 主题

### 下载主题
```sh
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

### 启用主题
修改 {% label primary@站点配置文件 %}，搜索 `theme` 字段。
```
theme: next
```

### 主题设定

#### 选择 Scheme
修改 {% label info@主题配置文件 %}，搜索 `scheme` 字段。
```
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

#### 设置头像
修改 {% label info@主题配置文件 %}，搜索 `avatar` 字段。
```
avatar: /images/bio-photo.jpg
```

#### favicon 设置

```
# For example, you put your favicons into `hexo-site/source/images` directory.
# Then need to rename & redefine they on any other names, otherwise icons from Next will rewrite your custom icons in Hexo.
favicon:
  small: /images/favicon-16x16.ico
  medium: /images/favicon-32x32.ico
  apple_touch_icon: /images/apple-touch-icon.png
  safari_pinned_tab: /images/favicon-logo.svg
```

#### 关键字设置

```
# Set default keywords (Use a comma to separate)
keywords: "xuchengpeng, Hexo, NexT"
```

#### 页脚设置

```
footer:
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  #since: 2015

  # Icon between year and copyright info.
  icon: user

  # If not defined, will be used `author` from Hexo main config.
  copyright:
  # -------------------------------------------------------------
  # Hexo link (Powered by Hexo).
  powered: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    version: false
  # -------------------------------------------------------------
  # Any custom text can be defined here.
  #custom_text: Hosted by <a target="_blank" rel="external nofollow" href="https://pages.coding.me"><b>Coding Pages</b></a>
```

### 主题配置

#### 添加标签页面

{% tabs 添加标签页面 %}
<!-- tab 新建标签页面 -->
{% codeblock lang:sh %}
cd your-hexo-site
hexo new page tags
{% endcodeblock%}
<!-- endtab -->
<!-- tab 修改页面内容 -->
修改刚新建的页面，内容如下：
{% codeblock lang:markdown %}
---
title: "标签"
date: 2017-11-23 10:49:56
type: "tags"
comments: false
---
{% endcodeblock %}
<!-- endtab -->
<!-- tab 添加标签菜单 -->
修改 {% label info@主题配置文件 %}，添加 `tags` 到 `menu` 中。
{% codeblock %}
menu:
  tags: /tags/ || tags
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

#### 添加分类页面

{% tabs 添加分类页面 %}
<!-- tab 新建分类页面 -->
{% codeblock lang:sh %}
cd your-hexo-site
hexo new page categories
{% endcodeblock %}
<!-- endtab -->
<!-- tab 修改页面内容 -->
修改刚新建的页面，内容如下：
{% codeblock lang:markdown %}
---
title: "分类"
date: 2017-11-23 10:49:18
type: "categories"
comments: false
---
{% endcodeblock %}
<!-- endtab -->
<!-- tab 添加分类菜单 -->
修改 {% label info@主题配置文件 %}，添加 `categories` 到 `menu` 中。
{% codeblock %}
menu:
  categories: /categories/ || th
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

#### 添加留言页面

{% tabs 添加留言页面 %}
<!-- tab 新建留言页面 -->
{% codeblock lang:sh %}
cd your-hexo-site
hexo new page guestbook
{% endcodeblock %}
<!-- endtab -->
<!-- tab 修改页面内容 -->
修改刚新建的页面，内容如下：
{% codeblock lang:markdown %}
---
title: "留言"
date: 2017-11-23 10:50:05
type: "guestbook"
comments: true
---

{% centerquote %}
凡人皆有一死，凡人皆需侍奉。
{% endcenterquote %}

{% endcodeblock %}
<!-- endtab -->
<!-- tab 添加留言菜单 -->
修改 {% label info@主题配置文件 %}，添加 `guestbook` 到 `menu` 中。
{% codeblock %}
menu:
  留言: /guestbook/ || commenting
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

#### 菜单设置
```
menu_settings:
  icons: true
  badges: false
```

#### 社交链接
```
social:
  Wuhan, China.: https://www.google.com/maps || location-arrow
  E-Mail: mailto:xucp@outlook.com || envelope
  GitHub: https://github.com/xuchengpeng || github
  Bitbucket: https://bitbucket.org/xuchengpeng/ || bitbucket
```

#### 友情链接
```
links_icon: link
links_title: 友情链接
links_layout: block
#links_layout: inline
links:
  HarmonyHu: http://harmonyhu.com/
  FreeSS: https://freess.cx/
  LeanCloud: https://leancloud.cn/
  NexT: https://github.com/theme-next/hexo-theme-next
```

#### 站点 License
```
creative_commons: by-nc-sa
```

#### 文章 License
```
# Declare license on posts
post_copyright:
  enable: true
  license: <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/" rel="external nofollow" target="_blank">CC BY-NC-SA 4.0</a>
```

#### 打赏
```
# Reward
#reward_comment: Donate comment here
wechatpay: /images/wechatpay.jpg
alipay: /images/alipay.jpg
#bitcoin: /images/bitcoin.png
```

#### 文章目录设置

```
# Table Of Contents in the Sidebar
toc:
  enable: true

  # Automatically add list number to toc.
  number: true

  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: true
```

#### 侧边栏设置

```
sidebar:
  # Sidebar Position, available value: left | right (only for Pisces | Gemini).
  position: left
  #position: right

  # Sidebar Display, available value (only for Muse | Mist):
  #  - post    expand on posts automatically. Default.
  #  - always  expand for all pages automatically
  #  - hide    expand only when click on the sidebar toggle icon.
  #  - remove  Totally remove sidebar including sidebar toggle.
  display: post
  #display: always
  #display: hide
  #display: remove

  # Sidebar offset from top menubar in pixels (only for Pisces | Gemini).
  offset: 12

  # Back to top in sidebar (only for Pisces | Gemini).
  b2t: false

  # Scroll percent label in b2t button.
  scrollpercent: true

  # Enable sidebar on narrow view (only for Muse | Mist).
  onmobile: false
```

#### 设置代码高亮主题
```
highlight_theme: normal
```

#### 调整内容区域显示的宽度

修改 {% label info@主题配置文件 %}：
```
custom_file_path:
  variables: source/_data/variables.styl
```
在 `variables.styl` 文件中填入以下内容：
```
// 修改成你期望的宽度
$content-desktop = 800px

// 当视窗超过 1600px 后的宽度
$content-desktop-large = 1000px
```
Pisces 和 Gemini 的修改方法为:
```
$main-desktop    = 85%
$content-desktop = calc(100% - 260px)
```

#### 标签设置

```
# Note tag (bs-callout).
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: flat
  icons: true
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0

# Label tag.
label: true

# Tabs tag.
tabs:
  enable: true
  transition:
    tabs: true
    labels: true
  border_radius: 0
```

### 第三方功能

#### 设置页面文章的篇数
```sh
npm install --save hexo-generator-index
npm install --save hexo-generator-archive
npm install --save hexo-generator-tag
```
修改 {% label primary@站点配置文件 %}，设定如下选项：
```
index_generator:
  per_page: 5

archive_generator:
  per_page: 20
  yearly: true
  monthly: true

tag_generator:
  per_page: 10
```

#### 设置RSS
1. 安装插件
{% codeblock lang:sh %}
npm install hexo-generator-feed --save
{% endcodeblock %}
2. 增加配置
修改 {% label primary@站点配置文件 %}：
{% codeblock %}
# 参数说明查看 README：https://github.com/hexojs/hexo-generator-feed
feed:
  type: atom
  path: atom.xml
# 文章数，0 为全部
  limit: 0
  hub:
# 是否包含文章内容
  content: true
{% endcodeblock %}

#### [添加分享功能](https://github.com/theme-next/theme-next-needmoreshare2)
1. 下载插件
```sh
git clone https://github.com/theme-next/theme-next-needmoreshare2 source/lib/needsharebutton
```
2. 修改 {% label info@主题配置文件 %}：
```
needmoreshare2:
  enable: true
  postbottom:
    enable: true
  float:
    enable: false
```

~~#### Gitalk评论支持
{% post_link gitalk-for-next 为NexT主题部署Gitalk评论插件 %}~~

#### [Valine评论系统](https://valine.js.org/)

Valine 是一款基于Leancloud的快速、简洁且高效的无后端评论系统。

1. 获取 App ID 和 App Key。
登录 [LeadCloud](https://leancloud.cn)，进入控制台后创建应用，进入应用，在 `设置` > `应用Key` 中获取 {% label info@App ID %} 和 {% label info@App Key%}。
2. 在应用中配置自己的安全域名。
3. 修改 {% label info@主题配置文件 %}：
```
# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
valine:
  enable: true
  appid: xxxxxxxxx # your leancloud application appid
  appkey: xxxxxxxxxx # your leancloud application appkey
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Leave your comments here... # comment box placeholder
  avatar: wavatar # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
```

#### 文章阅读次数统计

1. 获取 App ID 和 App Key。
登录 [LeadCloud](https://leancloud.cn)，进入控制台后创建应用，进入应用，在 `设置` > `应用Key` 中获取 {% label info@App ID %} 和 {% label info@App Key%}。
2. 进入应用，在 `存储` 中创建名称为 {% label warning@Counter %} 的 Class，权限设置为 {% label info@无限制 %}。
3. 修改 {% label info@主题配置文件 %}：
```
# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: xxxxxxxx #<app_id>
  app_key: xxxxxxxxxx #<app_key>
```

#### [Fancybox](https://github.com/theme-next/theme-next-fancybox3)
1. 下载插件
```sh
git clone https://github.com/theme-next/theme-next-fancybox3 source/lib/fancybox
```
2. 修改 {% label info@主题配置文件 %}：
```
fancybox: true
```

#### [设置背景动画](https://github.com/theme-next/theme-next-canvas-nest)
1. 下载插件
```sh
git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest
```
2. 修改 {% label info@主题配置文件 %}：
```
canvas_nest: true
```

#### [字数统计和阅读时长](https://github.com/theme-next/hexo-symbols-count-time)
1. 安装插件
```sh
npm install hexo-symbols-count-time --save
```
2. 修改 {% label primary@站点配置文件 %}：
```
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
```
3. 修改 {% label info@主题配置文件 %}：
```
symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: true
  awl: 5
  wpm: 200
```

#### [顶部加载进度条](https://github.com/theme-next/theme-next-pace)
1. 下载插件
```sh
git clone https://github.com/theme-next/theme-next-pace source/lib/pace
```
2. 修改 {% label info@主题配置文件 %}：
```
pace: true
pace_theme: pace-theme-flash
```

#### [Local Search](https://github.com/theme-next/hexo-generator-searchdb)
1. 安装插件
{% codeblock lang:sh %}
npm install hexo-generator-searchdb --save
{% endcodeblock %}
2. 增加配置
修改 {% label primary@站点配置文件 %}，添加如下配置：
{% codeblock %}
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
{% endcodeblock %}
3. 启用本地搜索
{% codeblock %}
local_search:
  enable: true
{% endcodeblock %}

## 验证主题
启动本地站点，并开启调试模式，命令是 `hexo server --debug`。
此时可以使用浏览器访问 http://localhost:4000 ，检查是否运行正确。

## 生成器
使用 Hexo 生成静态文件快速而且简单。
```sh
hexo generate
```

## 部署
安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。
```sh
npm install hexo-deployer-git --save
```
修改 {% label primary@站点配置文件 %}。
```
deploy:
  type: git
  repo: git@github.com:xuchengpeng/xuchengpeng.github.io.git
  branch: master
```
Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上。
```sh
hexo deploy
```

## 写作

### 修改文章模板

编辑 `scaffolds/post.md` 文件：
```markdown
---
title: {{ title }}
date: {{ date }}
udpated: {{ date }}
comments: true
categories:
tags:
---

<!-- more -->

```

### 多图展示

```markdown
---
title: picture-test
comments: true
date: 2017-12-27 09:08:03
udpated: 2017-12-27 09:08:03
type: "picture"
categories:
 - Picture
tags:
 - Picture Group
---

{% gp 4-2 %}
  {% img /images/bio-photo.jpg %}
  {% img /images/hust-logo.jpg %}
  {% img /images/pt-search-demo.jpg %}
  {% img /images/RJ-45-crystal-head.jpg %}
{% endgp %}
```

多图排版格式见 [iissnan/hexo-theme-next#295](https://github.com/iissnan/hexo-theme-next/issues/295)，参考 `theme/next/scripts/tags/group-pictures.js` 文件定义。

### Label 写作样式

```markdown
{% label default@label_default %}
{% label primary@label_primary %}
{% label success@label_success %}
{% label info@label_info %}
{% label warning@label_warning %}
{% label danger@label_danger %}
```

{% label default@label_default %}
{% label primary@label_primary %}
{% label success@label_success %}
{% label info@label_info %}
{% label warning@label_warning %}
{% label danger@label_danger %}

### Tab 写作样式

```markdown
{% tabs test %}
<!-- tab aaa -->
**这是选项卡 1** 呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈……
<!-- endtab -->
<!-- tab bbb -->
**这是选项卡 2**
<!-- endtab -->
<!-- tab ccc -->
**这是选项卡 3** 哇，你找到我了！φ(≧ω≦*)♪～
<!-- endtab -->
{% endtabs %}
```

{% tabs test %}
<!-- tab aaa -->
**这是选项卡 1** 呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈……
<!-- endtab -->
<!-- tab bbb -->
**这是选项卡 2**
<!-- endtab -->
<!-- tab ccc -->
**这是选项卡 3** 哇，你找到我了！φ(≧ω≦*)♪～
<!-- endtab -->
{% endtabs %}

### Note 写作样式

```
{% note default %}
note default
{% endnote %}

{% note primary %}
note primary
{% endnote %}

{% note success %}
note success
{% endnote %}

{% note info %}
note info
{% endnote %}

{% note warning %}
note warning
{% endnote %}

{% note danger %}
note danger
{% endnote %}
```

{% note default %}
note default
{% endnote %}

{% note primary %}
note primary
{% endnote %}

{% note success %}
note success
{% endnote %}

{% note info %}
note info
{% endnote %}

{% note warning %}
note warning
{% endnote %}

{% note danger %}
note danger
{% endnote %}

### Button 写作样式

```markdown
{% button https://www.baidu.com, 点击下载百度, download fa-lg fa-fw %}
```

{% button https://www.baidu.com, 点击下载百度, download fa-lg fa-fw %}

### 文字字体和颜色

```markdown
<font color=red size=6 face=“黑体”>字体颜色</font>
```

<font color=red size=6 face=“黑体”>字体颜色</font>

### 文字背景色

```markdown
<table><tr><td bgcolor=cyan>背景颜色</td></tr></table>
```

<table><tr><td bgcolor=cyan>背景颜色</td></tr></table>
