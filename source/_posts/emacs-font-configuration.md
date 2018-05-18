---
title: "emacs 字体配置"
date: 2018-05-18 14:02:30
udpated: 2018-05-18 14:02:30
comments: true
categories:
 - Software
 - emacs
tags:
 - emacs
---

定义设置中文和英文字体的函数，达到等宽效果：
```el
(defun set-font()
  (interactive)
  
  ;; Setting English Font
  (when (member "DejaVu Sans Mono" (font-family-list))
    (set-face-attribute 'default nil :font
                        (format "%s:pixelsize=%d" "DejaVu Sans Mono" 14))
    )
  
  ;; Setting Chinese font
  (dolist (charset '(kana han symbol cjk-misc bopomofo))
    (set-fontset-font (frame-parameter nil 'font)
                      charset
                      (font-spec :family "Microsoft Yahei" :size 16))
    )
  
  ;; Fix chinese font width and rescale
  (setq face-font-rescale-alist '(("STHeiti" . 1.2) ("STFangsong" . 1.2) ("Microsoft Yahei" . 1.2) ("WenQuanYi Micro Hei Mono" . 1.2)))
  )
```

调用 set-font 函数可使设置生效：
```el
(add-to-list 'after-make-frame-functions
             (lambda (new-frame)
               (select-frame new-frame)
               (if window-system
                   (set-font))))

(if window-system
    (set-font))
```
<!-- more -->

分别为中文和英文字体定义一个列表，系统中存在这个字体就设定：
```el
(defun dotemacs-font-existsp (font)
  (if (null (x-list-fonts font))
      nil
    t))

;; or
;; (defun dotemacs-font-existsp (font)
;;   "Detect if a font exists"
;;   (if (find-font (font-spec :family font))
;;         t
;;       nil))

(defun dotemacs-make-font-string (font-name font-size)
  (if (and (stringp font-size)
           (equal ":" (string (elt font-size 0))))
      (format "%s%s" font-name font-size)
    (format "%s %s" font-name font-size)))

(defun dotemacs-set-font (english-fonts
                       english-font-size
                       chinese-fonts
                       &optional chinese-font-size)

  "english-font-size could be set to \":pixelsize=18\" or a integer.
   If set/leave chinese-font-size to nil, it will follow english-font-size"
  (require 'cl) ; for find if
  (let ((en-font (dotemacs-make-font-string
                  (find-if #'dotemacs-font-existsp english-fonts)
                  english-font-size))
        (zh-font (font-spec :family (find-if #'dotemacs-font-existsp chinese-fonts)
                            :size chinese-font-size)))

    ;; Set English font
    ;; (message "Set English Font to %s" en-font)
    (set-face-attribute 'default nil :font en-font)

    ;; Set Chinese font
    ;; (message "Set Chinese Font to %s" zh-font)
    (dolist (charset '(kana han symbol cjk-misc bopomofo))
      (set-fontset-font (frame-parameter nil 'font)
                        charset zh-font)))
    
    ;; Fix chinese font width and rescale
    (setq face-font-rescale-alist '(("STHeiti" . 1.2) ("STFangsong" . 1.2) ("Microsoft Yahei" . 1.2) ("WenQuanYi Micro Hei Mono" . 1.2))))

(defun set-font()
  (interactive)
  (dotemacs-set-font
    '("DejaVu Sans Mono" "Monaco" "Source Code Pro" "Consolas") ":pixelsize=14"
    '("Microsoft Yahei" "文泉驿等宽微米黑" "黑体" "新宋体" "宋体") 16)
  )
```

为不同的操作系统设定不同的字体：
```el
(defun set-font()
  (interactive)
  (setq fonts
        (cond ((eq system-type 'darwin)     '("Monaco"           "STHeiti"))
              ((eq system-type 'gnu/linux)  '("Menlo"            "WenQuanYi Zen Hei"))
              ((eq system-type 'windows-nt) '("DejaVu Sans Mono" "Microsoft Yahei"))))
  (set-face-attribute 'default nil :font
                      (format "%s:pixelsize=%d" (car fonts) 14))
  (dolist (charset '(kana han symbol cjk-misc bopomofo))
    (set-fontset-font (frame-parameter nil 'font) charset
                      (font-spec :family (car (cdr fonts)) :size 16)))
  ;; Fix chinese font width and rescale
  (setq face-font-rescale-alist '(("STHeiti" . 1.2) ("STFangsong" . 1.2) ("Microsoft Yahei" . 1.2) ("WenQuanYi Micro Hei Mono" . 1.2)))
  )
```

