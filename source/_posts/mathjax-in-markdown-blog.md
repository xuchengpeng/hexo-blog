---
title: "Markdown 博客中使用 MathJax 数学公式"
date: 2018-05-25 16:28:02
udpated: 2018-05-25 16:28:02
comments: true
mathjax: true
categories:
 - Technology
 - Web
tags:
 - Markdown
 - MathJax
 - LaTeX
---

[MathJax](https://www.mathjax.org/) - Beautiful math in all browsers

> A JavaScript display engine for mathematics that works in all browsers. No more setup for readers. It just works.

## MathJax 使用

网页中增加配置：

```html
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        processEscapes: true
      }
    });
</script>

<script src='https://cdn.jsdelivr.net/npm/mathjax@2.7.4/MathJax.js?config=TeX-AMS-MML_HTMLorMML' async></script>
```
<!-- more -->

接下来就可以在Markdown中使用LaTeX代码来编写公式了。注意要用 `$`（行内代码）或 `$$`(代码块）来把数学代码和正常文字区分开。

如，`$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$`效果为$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$。

但要注意，Markdown 文件会把两个 `_` 符号当作下划线或斜体， _比如 这样_ ，因此当公式的代码中需要输入 `_` 符号时，应该在前面加 `\` 进行转义，如：
```latex
$$ evidence\_{i}=\sum\_{j}W\_{ij}x\_{j}+b\_{i} $$
```

效果为：
$$ evidence\_{i}=\sum\_{j}W\_{ij}x\_{j}+b\_{i} $$

## LaTeX 数学公式

1. [识别并转换数学公式的网站](https://webdemo.myscript.com/views/math.html)
2. [LaTeX Tutorial](http://www.forkosh.com/mathtextutorial.html)
3. [LaTeX 支持的数学符号表](http://get-software.net/info/symbols/math/maths-symbols.pdf)

