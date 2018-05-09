---
title: "Add shell-pop with eshell"
date: 2018-05-09 19:40:40
udpated: 2018-05-09 19:40:40
comments: true
categories:
 - Software
 - emacs
tags:
 - eshell
 - shell-pop
---

```el
(use-package shell-pop
  :ensure t
  :defer t
  :bind ("C-c t" . shell-pop)
  :config
  (custom-set-variables
   '(shell-pop-shell-type (quote ("eshell" "*eshell*" (lambda nil (eshell)))))
   '(shell-pop-window-size 30)
   '(shell-pop-full-span t)
   '(shell-pop-window-position "bottom"))
  )
```
<!-- more -->
