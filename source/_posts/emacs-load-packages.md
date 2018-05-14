---
title: "Emacs load packages"
date: 2018-05-11 13:51:45
udpated: 2018-05-11 13:51:45
comments: true
categories:
 - Software
 - emacs
tags:
 - require
 - autoload
 - with-eval-after-load
 - use-package
---

## load

load → is the general function for loading a file.

```el
(load FILE &optional NOERROR NOMESSAGE NOSUFFIX MUST-SUFFIX)
```

举个例子，如果你的代码是 `(load "x")`，emacs 将从 load-path 中尝试加载 x.elc、x.el、x 文件。

## load-file

load-file → load one specific file. The file name argument should contain file name extension, such as .el .elc

`(load-file file_name)` just calls `(load (expand-file-name file_name) nil nil t)`

Use load-file when you have a specific full path of a file in mind.

举个例子：
```el
load-file "~/.emacs.d/lisp/init-example.el"
```

这是最为原始的方式，填写的路径必须是绝对路径，这个路径也不会加入到emacs中load-path里。它也不会优先寻找编译过.elc文件（显然编译过文件的会更快些）。这种方式已经被抛弃，仅作为历史提一下。
<!-- more -->

## require

require → Load a package if it has not already been loaded.

```el
(require FEATURE &optional FILENAME NOERROR)
```

require checks if the symbol FEATURE is in variable features. If not, then it calls load to load it.

File name is guessed from the feature name FEATURE, or specified in the optional argument.

require is best used in elisp libraries or scripts, similar to other language's “require” or “import”.

举个例子：
*init-example.el*
```el
... your code here ...

(provide 'init-example)
```

*init.el*
```el
... your code here ...
(require 'init-example)
```

## autoload

autoload → Load a file only when a function is called.

```el
(autoload FUNCTION FILE &optional DOCSTRING INTERACTIVE TYPE)
```

autoload associates a function name with a file path. When the function is called, load the file, and execute the function.

If you are writing a major mode, have your package installation go by autoload if possible. It saves startup time.

举个例子：
```el
(autoload 'python-mode "python-mode" "Python Mode." t)
```

## with-eval-after-load

If you want code to be executed when a particular library is loaded, use the macro `with-eval-after-load`.

`with-eval-after-load` is like the old `eval-after-load`, but better behaved.

eval-after-load is considered ill-behaved because it is a function, not a macro, and thus requires the code inside it to be quoted, which means that it cannot be byte-compiled. It also accepts only one form, so if you have more than one, you need to use progn. For example:
```el
(eval-after-load "foo"
  '(progn
     (setq foo 42)
     (setq bar 17)))
```
The equivalent version with with-eval-after-load would be:
```el
(with-eval-after-load "foo"
  (setq foo 42)
  (setq bar 17))
```

Define a macro `after-load` which is compatible with both `with-eval-after-load` and `eval-after-load`.
```el
(if (fboundp 'with-eval-after-load)
    (defalias 'after-load 'with-eval-after-load)
  (defmacro after-load (feature &rest body)
    "After FEATURE is loaded, evaluate BODY."
    (declare (indent defun))
    `(eval-after-load ,feature
       '(progn ,@body))))
```

## [use-package](https://github.com/jwiegley/use-package)

The `use-package` macro allows you to isolate package configuration in your .emacs file in a way that is both performance-oriented and, well, tidy. For more information, visit https://jwiegley.github.io/use-package .

