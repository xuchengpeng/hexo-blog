---
title: "Search with ripgrep"
date: 2018-03-17 17:48:44
udpated: 2018-06-01 17:46:44
comments: true
categories:
- Software
- Search
tags:
- ripgrep
- emacs
- helm-ag
---

[ripgrep](https://github.com/BurntSushi/ripgrep) is a line-oriented search tool that recursively searches your current directory for a regex pattern while respecting your gitignore rules. ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release. ripgrep is similar to other popular search tools like The Silver Searcher, ack and grep.

## Is it really faster than everything else?

Generally, yes. A large number of benchmarks with detailed analysis for each is [available on my blog](http://blog.burntsushi.net/ripgrep/).

Summarizing, ripgrep is fast because:

* It is built on top of [Rust's regex engine](https://github.com/rust-lang-nursery/regex). Rust's regex engine uses finite automata, SIMD and aggressive literal optimizations to make searching very fast.
* Rust's regex library maintains performance with full Unicode support by building UTF-8 decoding directly into its deterministic finite automaton engine.
* It supports searching with either memory maps or by searching incrementally with an intermediate buffer. The former is better for single files and the latter is better for large directories. ripgrep chooses the best searching strategy for you automatically.
* Applies your ignore patterns in `.gitignore` files using a [`RegexSet`](https://doc.rust-lang.org/regex/regex/struct.RegexSet.html). That means a single file path can be matched against multiple glob patterns simultaneously.
* It uses a lock-free parallel recursive directory iterator, courtesy of [`crossbeam`](https://docs.rs/crossbeam) and [`ignore`](https://docs.rs/ignore).
<!-- more -->

## Benchmark

This example searches the entire Linux kernel source tree (after running make defconfig && make -j8) for [A-Z]+_SUSPEND, where all matches must be words. Timings were collected on a system with an Intel i7-6900K 3.2 GHz, and ripgrep was compiled with SIMD enabled.

| Tool | Command | Line count | Time |
| ---- | ------- | ---------- | ---- |
| [ripgrep](https://github.com/BurntSushi/ripgrep) (Unicode) | `rg -n -w '[A-Z]+_SUSPEND'` | 450 | **0.106s** |
| [git grep](https://www.kernel.org/pub/software/scm/git/docs/git-grep.html) | `LC_ALL=C git grep -E -n -w '[A-Z]+_SUSPEND'` | 450 | 0.553s |
| [The Silver Searcher](https://github.com/ggreer/the_silver_searcher) | `ag -w '[A-Z]+_SUSPEND'` | 450 | 0.589s |
| [git grep (Unicode)](https://www.kernel.org/pub/software/scm/git/docs/git-grep.html) | `LC_ALL=en_US.UTF-8 git grep -E -n -w '[A-Z]+_SUSPEND'` | 450 | 2.266s |
| [sift](https://github.com/svent/sift) | `sift --git -n -w '[A-Z]+_SUSPEND'` | 450 | 3.505s |
| [ack](https://github.com/petdance/ack2) | `ack -w '[A-Z]+_SUSPEND'` | 1878 | 6.823s |
| [The Platinum Searcher](https://github.com/monochromegane/the_platinum_searcher) | `pt -w -e '[A-Z]+_SUSPEND'` | 450 | 14.208s |

Here's another benchmark that disregards gitignore files and searches with a whitelist instead. The corpus is the same as in the previous benchmark, and the flags passed to each command ensure that they are doing equivalent work:

| Tool | Command | Line count | Time |
| ---- | ------- | ---------- | ---- |
| ripgrep | `rg -L -u -tc -n -w '[A-Z]+_SUSPEND'` | 404 | **0.079s** |
| [ucg](https://github.com/gvansickle/ucg) | `ucg --type=cc -w '[A-Z]+_SUSPEND'` | 390 | 0.163s |
| [GNU grep](https://www.gnu.org/software/grep/) | `egrep -R -n --include='*.c' --include='*.h' -w '[A-Z]+_SUSPEND'` | 404 | 0.611s |

And finally, a straight-up comparison between ripgrep and GNU grep on a single large file (~9.3GB, [OpenSubtitles2016.raw.en.gz](http://opus.lingfil.uu.se/OpenSubtitles2016/mono/OpenSubtitles2016.raw.en.gz)):

| Tool | Command | Line count | Time |
| ---- | ------- | ---------- | ---- |
| ripgrep | `rg -w 'Sherlock [A-Z]\w+'` | 5268 | **2.108s** |
| [GNU grep](https://www.gnu.org/software/grep/) | `LC_ALL=C egrep -w 'Sherlock [A-Z]\w+'` | 5268 | 7.014s |

## helm-ag with rg

```el
(use-package helm-ag
  :ensure t
  :after (helm)
  :config
  (setq helm-ag-base-command "rg --color=always --smart-case --no-heading --line-number")
  )
```

## helm-do-grep-ag

```el
(setq helm-grep-ag-command "rg --color=always --smart-case --no-heading --line-number %s %s %s")
```
