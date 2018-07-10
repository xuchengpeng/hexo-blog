---
title: "Setting MathJax with Hugo"
date: 2018-07-10 15:15:00
udpated: 2018-07-10 15:15:00
comments: true
mathjax: true
categories:
 - Technology
 - Web
tags:
 - Markdown
 - MathJax
 - Hugo
---

[MathJax](https://www.mathjax.org/) - Beautiful math in all browsers

> A JavaScript display engine for mathematics that works in all browsers. No more setup for readers. It just works.

<!--more-->

## The code of MathJax in Hugo theme

Create a file named `mathjax_support.html` in the `/layout/partials/` folder. Insert the below code into it, for example.

```html
<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@2.7.4/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre','code'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>
```

Then, include `mathjax_support.html` in `footer.html` or `header.html`. In my case. I chose to put it in `header.html` as follows:

```html
{{ if .Params.mathjax}}{{ partial "mathjax_support.html" . }}{{ end }}
```

This code means `mathjax_support.html` will be include as long as you add `mathjax = ture` to your front matter.

## Using MathJax in your post

First, add `mathjax = ture` to your front matter. It could be like this.

```markdown
+++
title = "Setting MathJax with Hugo"
date = 2018-07-10T14:28:41+08:00
draft = false
mathjax = true
toc = true
categories = [ "Technology", "Web" ]
tags = [ "MathJax", "Hugo" ]
+++
```

Second, write LaTeX code in your post, put inline math into `'$','$'`, and display math between `'$$','$$'`.

## Inline math

Here is inline math: `$\sqrt{3x-1}+(1+x)^2$`

The result is: $\sqrt{3x-1}+(1+x)^2$

## Display math

Here is display math:

```
$$
\begin{align}
  \nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \newline
  \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \newline
  \nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \newline
  \nabla \cdot \vec{\mathbf{B}} & = 0
\end{align}
$$
```

The result is:
$$
\begin{align}
  \nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \newline
  \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \newline
  \nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \newline
  \nabla \cdot \vec{\mathbf{B}} & = 0
\end{align}
$$

## Subscript issue with markdown

After enabling MathJax, any math entered between proper markers (see the MathJax documentation) will be processed and typeset in the web page. One issue that comes up, however, with Markdown is that the underscore character (`_`) is interpreted by Markdown as a way to wrap text in emph blocks while LaTeX (MathJax) interprets the underscore as a way to create a subscript. This “double speak” of the underscore can result in some unexpected and unwanted behavior.

There are many solutions to the problem, the [Hugo official document](https://gohugo.io/content-management/formats/#mathjax-with-hugo) hints a way to make MathJax work. It revolves a series of complex steps to correctly display LaTeX code. And you have to put inline mathematics between `'$ TeX Code $'`, and put display style mathematics between `<div>$$TeX Code$$</div>`. This method treats ‘inline math’ with ‘code’, which may get some trouble in your website style.

I think it’s too complex and tedious, my suggestion is to simply use `\_` instead of `_` in your LaTeX code.

Here is a example: `$\sum\_{k=1}^n a\_k$`

The result is $\sum\_{k=1}^n a\_k$.

