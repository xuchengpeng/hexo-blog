---
title: "Emacs add yasnippet to company backends"
date: 2018-04-27 10:10:07
udpated: 2018-04-27 10:10:07
comments: true
categories:
 - Software
 - emacs
tags:
 - yasnippet
 - company
---

```el
(defvar company-mode/enable-yas t
  "Enable yasnippet for all backends.")

(defun company-mode/backend-with-yas (backend)
  (if (or (not company-mode/enable-yas) (and (listp backend) (member 'company-yasnippet backend)))
      backend
    (append (if (consp backend) backend (list backend))
            '(:with company-yasnippet))))

(setq company-backends (mapcar #'company-mode/backend-with-yas company-backends))
```
<!-- more -->
