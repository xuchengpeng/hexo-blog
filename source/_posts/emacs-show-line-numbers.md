---
title: "Emacs: Show Line Numbers"
comments: true
date: 2018-03-30 09:52:44
udpated: 2018-03-30 09:52:44
categories:
 - Software
 - emacs
tags:
 - emacs
---

emacs has 2 line numbers mode:

* `Alt + x` linum-mode -> old, hack, slow. Emacs 23.
* `Alt + x` global-display-line-numbers-mode -> Emacs 26.

## global-display-line-numbers-mode

Emacs 26 has a new line number mode.

* global-display-line-numbers-mode -> show line numbers in all buffers.
* display-line-numbers-mode -> show line numbers in current buffers.

Put this in your emacs init file:
```el
(when (version<= "26.0.50" emacs-version )
  (global-display-line-numbers-mode))
```
<!-- more -->
display-line-numbers-mode is written by Eli Zaretskii.

## linum-mode

Emacs 23 has a line number mode. It shows line numbers in margin.

* linum-mode -> toggle line number in current.
* global-linum-mode -> toggle line number in all buffers.

To set it permanetnly, put this in your emacs init file:
```el
(global-linum-mode 1)
```

## nlinum (Recommended)

As linum-mode is slow, you can use nlinum instead of linum-mode.

```el
(if (version< emacs-version "26")
    (use-package nlinum
      :ensure t
      :config
      (global-nlinum-mode 1))
  (global-display-line-numbers-mode))
```
