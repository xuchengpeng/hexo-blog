---
title: "Add Emacs elpa configuration"
date: 2018-03-06 18:56:50
updated: 2018-03-07 16:07:50
comments: true
categories:
 - Software
 - emacs
tags:
 - emacs
 - elpa
 - use-package
 - require-package
---

## ELPA mirror

### Emacs China镜像

| ELPA                   | 镜像地址                                       |
|------------------------|------------------------------------------------|
| [GNU ELPA](http://elpa.gnu.org/)              | http://elpa.emacs-china.org/gnu/               |
| [MELPA](https://melpa.org/)                  | http://elpa.emacs-china.org/melpa/             |
| [MELPA Stable](http://stable.melpa.org/#/)           | http://elpa.emacs-china.org/melpa-stable/      |
| [Marmalade](Https://marmalade-repo.org/)              | http://elpa.emacs-china.org/marmalade/         |
| [Org](http://orgmode.org/elpa.html)                    | http://elpa.emacs-china.org/org/               |
| [Sunrise Commander ELPA](https://www.emacswiki.org/emacs/Sunrise_Commander) | http://elpa.emacs-china.org/sunrise-commander/ |
| [user42 ELPA](http://user42.tuxfamily.org/elpa/index.html)            | http://elpa.emacs-china.org/user42/            |

如果需要 HTTPS，请将镜像地址中的 `http` 改成 `https` 。

### 清华镜像

| ELPA              | 镜像地址                                    |
|-------------------|---------------------------------------------|
| [GNU ELPA](http://elpa.gnu.org/)          | http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/          |
| [MELPA](https://melpa.org/)             | http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/        |
| [MELPA Stable](http://stable.melpa.org/#/)      | http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa-stable/ |
| [Marmalade](https://marmalade-repo.org/)         | http://mirrors.tuna.tsinghua.edu.cn/elpa/marmalade/    |
| [Org](http://orgmode.org/elpa.html)               | http://mirrors.tuna.tsinghua.edu.cn/elpa/org/          |

*可以从 [d12frosted/elpa-mirror](https://github.com/d12frosted/elpa-mirror) 下载包镜像到本地使用。*
<!--more -->
## 包管理

定义如下代码，即可使用 `use-package` 或 `require-package` 来安装和管理包：
```el
(defun require-package (package &optional min-version no-refresh)
  "Install given PACKAGE, optionally requiring MIN-VERSION.
If NO-REFRESH is non-nil, the available package lists will not be
re-downloaded in order to locate PACKAGE."
  (if (package-installed-p package min-version)
      t
    (if (or (assoc package package-archive-contents) no-refresh)
        (if (boundp 'package-selected-packages)
            ;; Record this as a package the user installed explicitly
            (package-install package nil)
          (package-install package))
      (progn
        (package-refresh-contents)
        (require-package package min-version t)))))


(defun maybe-require-package (package &optional min-version no-refresh)
  "Try to install PACKAGE, and return non-nil if successful.
In the event of failure, return nil and print a warning message.
Optionally require MIN-VERSION.  If NO-REFRESH is non-nil, the
available package lists will not be re-downloaded in order to
locate PACKAGE."
  (condition-case err
      (require-package package min-version no-refresh)
    (error
     (message "Couldn't install optional package `%s': %S" package err)
     nil)))

(setq package-archives
      '(("melpa" . "E:/GitHub/elpa-mirror/melpa")
        ("org"   . "E:/GitHub/elpa-mirror/org")
        ("gnu"   . "E:/GitHub/elpa-mirror/gnu")
       )
)

;; update the package metadata is the local cache is missing
(unless package-archive-contents
  (package-refresh-contents))

(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))

(eval-when-compile
  (require 'use-package))
```
*请注意修改 package-archives 中的 ELPA 源。*

## 使用示例

### use-package

```el
(use-package helm
  :ensure t
  )
```

### require-package

```el
(require-package 'helm)
```
