---
title: "自定义emacs mode-line"
date: 2018-07-09 09:04:34
udpated: 2018-07-09 09:04:34
comments: true
mathjax: false
categories:
 - Software
 - emacs
tags:
 - emacs
---

## 一些常用的mode-line包

* [powerline](https://github.com/milkypostman/powerline)
* [smart-mode-line](https://github.com/Malabarba/smart-mode-line)
* [spaceline](https://github.com/TheBB/spaceline)
* [telephone-line](https://github.com/dbordak/telephone-line)

## 了解mode-line

mode-line初始值：
```el
("%e" mode-line-front-space mode-line-mule-info mode-line-client mode-line-modified mode-line-remote mode-line-frame-identification mode-line-buffer-identification "   " mode-line-position
 (vc-mode vc-mode)
 "  " mode-line-modes mode-line-misc-info mode-line-end-spaces)
```

<!-- more -->

### mode-line变量

* mode-line-mule-info：显示当前语言环境、编码信息和输入法；
* mode-line-modified：显示当前内容是否被修改，`**`表示已经被修改，`--`表示没有被修改，`%%`表示只读，`%*`表示只读但是被修改；
* mode-line-frame-identification：用来标识当前窗口；
* mode-line-buffer-identification：用来表示当前buffer；
* mode-line-position：用来表示在buffer中的位置，可显示位置百分比、buffer大小、当前行号和列号；
* mode-line-percent-position：在`mode-line-position`中被使用；
* vc-mode：显示版本管理信息；
* mode-line-modes：显示当前major和minor modes；
* mode-line-remote：用来显示当前buffer的default-directory是否是远程文件夹；
* mode-line-client：用来表示emacsclient窗口；
* mode-name： 显示当前buffer的major mode；
* mode-line-front-space：显示在mode-line最前面；
* mode-line-end-spaces：显示在mode-line最后面；
* mode-line-misc-info：显示杂项信息；
* minor-mode-alist：包含当前buffer激活的所有minor modes；
* global-mode-string：This variable holds a mode line construct that, by default, appears in the mode line just after the which-func-mode minor mode if set, else after mode-line-modes. The command display-time sets global-mode-string to refer to the variable display-time-string, which holds a string containing the time and load information. The ‘%M’ construct substitutes the value of global-mode-string, but that is obsolete, since the variable is included in the mode line from mode-line-format.

### %-Constructs in the Mode Line

* %b：当前buffer名称；
* %c：当前列号，从最左边0开始计算；
* %C：当前列号，从最左边1开始计算；
* %e：When Emacs is nearly out of memory for Lisp objects, a brief message saying so. Otherwise, this is empty.
* %f：文件名；
* %F：窗口名称；
* %i：当前buffer大小；
* %I：Like ‘%i’, but the size is printed in a more readable way by using ‘k’ for 10^3, ‘M’ for 10^6, ‘G’ for 10^9, etc., to abbreviate.
* %l：当前行号；
* %n：‘Narrow’ when narrowing is in effect; nothing otherwise.
* %o：The degree of travel of the window through (the visible portion of) the buffer, i.e. the size of the text above the top of the window expressed as a percentage of all the text outside the window, or ‘Top’, ‘Bottom’ or ‘All’.
* %p：The percentage of the buffer text above the top of window, or ‘Top’, ‘Bottom’ or ‘All’. Note that the default mode line construct truncates this to three characters.
* %P：The percentage of the buffer text that is above the bottom of the window (which includes the text visible in the window, as well as the text above the top), plus ‘Top’ if the top of the buffer is visible on screen; or ‘Bottom’ or ‘All’.
* %q：The percentages of text above both the top and the bottom of the window, separated by ‘-’, or ‘All’.
* %s：The status of the subprocess belonging to the current buffer, obtained with process-status.
* %z：The mnemonics of keyboard, terminal, and buffer coding systems.
* %Z：Like ‘%z’, but including the end-of-line format.
* %\*：‘%’ if the buffer is read only (see buffer-read-only); ‘\*’ if the buffer is modified (see buffer-modified-p); ‘-’ otherwise.
* %+：‘\*’ if the buffer is modified (see buffer-modified-p); ‘%’ if the buffer is read only (see buffer-read-only); ‘-’ otherwise. This differs from ‘%\*’ only for a modified read-only buffer.
* %&：‘\*’ if the buffer is modified, and ‘-’ otherwise.
* %[：An indication of the depth of recursive editing levels (not counting minibuffer levels): one ‘[’ for each editing level.
* %]：One ‘]’ for each recursive editing level (not counting minibuffer levels).
* %-：Dashes sufficient to fill the remainder of the mode line.
* %%：The character ‘%’—this is how to include a literal ‘%’ in a string in which %-constructs are allowed.
* %m：mode-name；
* %M：global-mode-string；

## 自定义mode-line

修改`mode-line-format`的值：
```el
(defun dotemacs-mode-line-fill (face reserve)
  "Return empty space using FACE and leaving RESERVE space on the right."
  (unless reserve
    (setq reserve 20))
  (when (and (display-graphic-p) (eq 'right (get-scroll-bar-mode)))
    (setq reserve (- reserve 3)))
  (propertize " "
              'display `((space :align-to
                                (- (+ right right-fringe right-margin) ,reserve)))
              'face face))

(defun dotemacs-buffer-encoding-abbrev ()
  "The line ending convention used in the buffer."
  (let ((buf-coding (format "%s" buffer-file-coding-system)))
    (if (string-match "\\(dos\\|unix\\|mac\\)" buf-coding)
        (match-string 1 buf-coding)
      buf-coding)))

(setq-default mode-line-format
                (list
                 "%e"
                 mode-line-front-space
                 ;; mode-line-mule-info
                 ;; mode-line-client
                 ;; mode-line-modified
                 ;; mode-line-remote
                 ;; mode-line-frame-identification
                 " "
                 ;; mode-line-buffer-identification
                 '(:eval (propertize "%b" 'face 'font-lock-keyword-face
                                     'help-echo (buffer-file-name)))
                 
                 " [" ;; insert vs overwrite mode, input-method in a tooltip
                 '(:eval (propertize (if overwrite-mode "Ovr" "Ins")
                                     'face 'font-lock-preprocessor-face
                                     'help-echo (concat "Buffer is in "
                                                        (if overwrite-mode
                                                            "overwrite"
                                                          "insert") " mode")))

                 ;; was this buffer modified since the last save?
                 '(:eval (when (buffer-modified-p)
                           (concat "," (propertize "Mod"
                                                   'face 'font-lock-warning-face
                                                   'help-echo "Buffer has been modified"))))

                 ;; is this buffer read-only?
                 '(:eval (when buffer-read-only
                           (concat "," (propertize "RO"
                                                   'face 'font-lock-type-face
                                                   'help-echo "Buffer is read-only"))))
                 "] "
                 
                 "["
                 (propertize "%p" 'face 'font-lock-constant-face)
                 "/"
                 (propertize "%I" 'face 'font-lock-constant-face)
                 "] "
                 
                 mode-line-modes
                 
                 "   "
                 '(:eval `(vc-mode vc-mode))
                 "   "
                 
                 ;; (dotemacs-mode-line-fill 'mode-line 35)
                 
                 ;;mode-line-position
                 " ("
                 (propertize "%l" 'face 'font-lock-type-face)
                 ","
                 (propertize "%c" 'face 'font-lock-type-face)
                 ") "
                 
                 '(:eval (dotemacs-buffer-encoding-abbrev))
                 "  "
                 '(:eval mode-line-misc-info)
                 
                 mode-line-end-spaces
                 ))
```

## 隐藏mode-line上的minor mode

使用[diminish](https://github.com/emacsmirror/diminish)：
```el
(require 'diminish)
;; Hide jiggle-mode lighter from mode line
(diminish 'jiggle-mode)
;; Replace abbrev-mode lighter with "Abv"
(diminish 'abbrev-mode "Abv")
```

