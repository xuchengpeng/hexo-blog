---
title: 使用 Hexo 数据文件进行主题配置
date: 2018-01-19 07:41:32
udpated: 2018-01-31 15:42:32
comments: true
categories:
 - Technology
 - Hexo
tags:
 - Hexo
---

目前，通过 pull 或下载新的 release 版本来更新 NexT 主题的体验并不平滑。当用户使用 git pull 更新 NexT 主题时经常需要解决冲突问题，而在手动下载 release 版本时也经常需要手动合并配置。

现在来说，NexT 推荐用户存储部分配置在站点的 _config.yml 中，而另一部分在主题的 _config.yml 中。这一方式固然可用，但也有一些缺点：
1. 配置项被分裂为两部分；
2. 用户难以弄清何处存放配置选项。

为了解决这一问题，NexT 将利用 Hexo 的[数据文件](https://hexo.io/docs/data-files.html)特性。因为数据文件是在 Hexo 3 中被引入，所以你需要更新至 Hexo 3.0 以后的版本来使用这一特性。

如果你仍然希望使用 Hexo 2.x，你依旧可以按老的方式进行配置。NexT 仍然兼容 Hexo 2.x（但可能会出现错误）。

<!-- more -->

## Hexo 的方式

使用这种方法，所有的配置保存在 hexo 配置文件中（`hexo/_config.yml`），并且不需要修改 `next/_config.yml` 配置文件或者创建其他任何文件。但是你必须在 `theme_config` 选项中保留双重空格缩进。

如果在新版本中有任何新的配置项，你只需要从 `next/_config.yml` 拷贝这些配置项，粘贴到 `hexo/_config.yml` 并且设定他们为你想要的值。

### 使用方法

1. 检查不存在 `hexo/source/_data/next.yml` 文件（如果存在就删除）。
2. 从 `next/_config.yml` 拷贝需要的 NexT 主题配置项到 `hexo/_config.yml` 中，然后：
   2.1. 把这些选项向右移动两个空格。
   2.2. 在这些选项的上方加上 `theme_config` 参数。

### 相关链接

* [Hexo Configuration](https://hexo.io/docs/configuration.html)
* [Hexo Pull #757](https://github.com/hexojs/hexo/pull/757)

## NexT 的方式

使用这种方法，你可以将所有的配置置于同一位置（`source/_data/next.yml`），并且不需要修改 `next/_config.yml`。但是这种选项可能无法准确的处理所有的 hexo 外部库及其附加选项（举个例子，`hexo-server` 模块选项可能只从默认的 hexo 配置中读取）。

如果在新版本中有任何新的配置项，你只需要从 `next/_config.yml` 拷贝这些配置项，粘贴到 `data/next.yml` 并且设定他们为你想要的值。

### 使用方法

1. 请确认你的 Hexo 版本为 3.0 或更高。
2. 在你站点的 `hexo/source/_data` 目录创建一个 `next.yml` 文件（如果 `_data` 目录不存在，请创建之）。
   在这些步骤之后有两个选择，只需要选择其中一个，然后继续下一步。
   * 选择1：`override: false` (default)：
     i. 检查 NexT 默认配置中的 `override` 选项，必须设定为 `false`。在 `next.yml` 中，无需定义它，或者也必须设定为 `false`。
     ii. 复制你站点的 `_config.yml` 和主题的 `_config.yml` 中的 **NexT 配置项** 到 `hexo/source/_data/next.yml` 中。
   * 选择2：`override: true`：
     i. 在 `next.yml` 中，`override` 选项必须设定为 `true`。
     ii. 复制你主题 `_config.yml` 中的 **所有配置项** 到 `hexo/source/_data/next.yml` 中。
3. 站点配置文件 `_config.yml` 中需要定义 `theme: next` 选项。
4. 使用标准参数来启动服务器，生成或部署（`hexo clean && hexo g -d && hexo s`）。

### 相关链接

* [NexT Issue #328](https://github.com/iissnan/hexo-theme-next/issues/328)

