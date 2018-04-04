---
title: "Speed up emacs startup time"
comments: true
date: 2018-03-28 09:11:50
updated: 2018-03-31 10:30:50
categories:
 - Software
 - emacs
tags:
 - emacs
---

*init.el*
```el
(setq package-enable-at-startup nil)

(defvar file-name-handler-alist-old file-name-handler-alist)

(setq file-name-handler-alist nil
      gc-cons-threshold 402653184
      gc-cons-percentage 0.6)

(add-hook 'after-init-hook
          (lambda ()
            (setq file-name-handler-alist file-name-handler-alist-old
                  gc-cons-threshold 16777216
                  gc-cons-percentage 0.1)))

...your code here...

(provide 'init)
```
<!-- more -->
## use-package 延迟加载的只言片语

Emacs lisp 有一项 auto-load 的技术，类似延迟加载，合理运用延迟，让笔者的 Emacs 启动加载时间减少一半，因为笔者用 use-package 了这个插件,而 use-package 又集 成了延迟加载的功能，所以笔者就直接拿自己的代码举例了

:after
```el
;;; Export to twitter bootstrap
(use-package ox-twbs
  :after org
  :ensure ox-twbs
  )
```
:after 关键字的作用基本跟 with-eval-after-load 的作用是相同的，所以笔者所 有类似的org-mode 插件包都会在org-mode 加载以后才会加载

:commands
```el
(use-package avy
  :commands (avy-goto-char avy-goto-line)
  :ensure t)
```
这里就直接贴上 use-package文档的说明了

> When you use the :commands keyword, it creates autoloads for those commands and defers loading of the module until they are used

也就是 :commands 关键字就创建了后面所接的命令的 autoloads 机制了

:bind :mode
```el
(use-package hi-lock
  :bind (("M-o l" . highlight-lines-matching-regexp)
	 ("M-o r" . highlight-regexp)
	 ("M-o w" . highlight-phrase)))

(use-package vue-mode
  :ensure t
  :mode ("\\.vue\\'" . vue-mode)
  :config (progn
	    (setq mmm-submode-decoration-level 0)
	    ))
```
附上文档说明

> In almost all cases you don't need to manually specify :defer t. This is implied whenever :bind or :mode or :interpreter is used

也就是说，当你使用了 :bind 或者 :mode 关键字的时候，不用明确指定 :defer 也可以实现延迟加载机制。当然你也可以，直接使用 :defer 关键字来指定延迟加载 不过前提是，你要明确它加载的时机

> Typically, you only need to specify :defer if you know for a fact that some other package will do something to cause your package to load at the appropriate time, and thus you would like to defer loading even though use-package isn't creating any autoloads for you.

贴上笔者自己的代码，可以更加清晰
```el
(use-package anaconda-mode
  :defer t
  :ensure t
  :init(progn
	 (add-hook 'python-mode-hook 'anaconda-mode)
	 (add-hook 'python-mode-hook 'anaconda-eldoc-mode)
	 )
```
这样 anaconda-mode 就会在 python-mode 加载以后被加载
