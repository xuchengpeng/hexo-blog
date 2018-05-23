---
title: "Stylus 自定义网页字体"
date: 2018-05-23 10:27:11
udpated: 2018-05-23 10:27:11
comments: true
categories:
 - Technology
 - Web
tags:
 - Stylus
 - Stylish
 - CSS
---

使用 Stylus 可以直接使用 [userstyles 网站](https://userstyles.org/) 提供的模板，很多下载后几乎不用设定即可使用，前提是现在电脑上安装相应的字体。

这里推荐一款 [ForceMyFont](https://userstyles.org/styles/100473/004-forcemyfonts-chinese-firefoxchromeie-catcat520) 的模板，可以设定的选项非常丰富，前提也是需要在电脑上安装你选择的字体。

你也可以自定义字体CSS的样式，在 Chrome 浏览器中，这里使用 Stylus 插件，编写新的样式，代码如下：
```css
*{font-weight:500!important;}
*{font-family: Arial, "思源宋体"!important;}
```
第一行设定字重，第二行设定英文字体和中文字体。如果要添加字体阴影，可以添加以下代码：
```css
*{text-shadow:0.01em 0.01em 0.01em #999999 !important;}
```
其中的数值依次代表阴影的 X 轴偏移、Y 轴偏移、阴影大小，以及字体颜色。
<!-- more -->

