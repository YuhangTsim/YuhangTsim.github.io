---
title: 从零开始搭建Github Page -- 网站页面、内容、功能的添加
date: 2018-05-22 21:09:01
tags: [Web, Github Page]
categories: Github page
---
在完成了网站的搭建、源码备份等工作后,我们可以开始进行网站内容、功能的设置。
<!-- more -->

# 前言

虽然已经成功部署了网页，但是网页内绝大多数内容都是网页模板的默认设置，非常多的内容和功能是缺失的。因此需要对其进行补充和添加。

区别于Wordpress，简书等平台，在Github Page添加各种内容和功能完全是自由的，而且Hexo也提供了丰富的插件供我们使用。由于暂时我还不需要太多的功能，因为本文只作对网站缺失内容的补充和搜索功能的添加，目的是使其成为一个完整的网站。

# 网站内容

在Next主题的主页中，默认开启了分类（category），归档（archives），标签（tags）三个页面。但是默认配置下，并不存在这三个页面所对应的网页。因此需要对其进行补充。

添加网页的命令如下：

```bash
$ hexo new page 'page_name'
```

该命令会在 ``/source``文件夹下生成新的``'page_name'``文件夹。文件夹内包含``index.md``，用于配置页面。

同时，需要在主题配置文件``/themes/config.yml``中开启分类、归档、和标签页面：

```yml
# Menu Settings
menu:
  home: / || home
  tags: /tags/ || tags
  categories: /categories/ || th
```

## 分类 Categories

在添加 __categories__ 页面之后,修改其对应的主页文件``index.md``如下：

```md
---
title: Categories
date: 2018-05-14 23:44:42
type: "categories"
---
```

## 标签 Tags

Tags对应的主页如下：

```yml
---
title: Tags
date: 2018-05-13 21:54:46
type: 'tags'
---
```

## 归档 Archives

归档页面是不用我们手动生成的，但是需要安装相应的插件：

```bash
$ npm install hexo-generator-archive --save
```

## Post的设置

添加完上述两个页面后，主要在每个post总添加对应的tags或categor，Hexo就能自动帮我们把相应的tags或categories添加到对应的页面。

比如本文的设置如下：

```yml
---
title: 从零开始搭建Github Page -- 网站页面、内容、功能的添加
date: 2018-05-22 21:09:01
tags: [Web, Github Page] # 多个tags放在[]内，并用‘，’间隔
categories: Github page
---
```

# 站内搜索 Local Search

搜索功能是每个网站都不可或缺的功能，方便读者快速的找的他们所感兴趣的话题。在Hexo中添加搜索功能首先需要安装对应的搜索插件：

```bash
$ npm install --save hexo-generator-search
```

而该插件默认只能搜索posts，为了实现关于页面的搜索需要在站点配置文件 ``/config.yml`` 中做相应的修改：

```yml
# search
search:
  path: search.xml
  field: all
  format: html
  limit: 10000
```

同是还需要在主题配置文件``/themes/config.yml``中启动Local search：

```yml
# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
local_search:
  enable: true
```

# 网站分析 Google Analysis

为了分析网站的流量，可以使用网站分析服务。主流的服务主要有Google Analysis，百度等，由于我目前使用GA最方便，此处以GA为例。

在主题配置文件 ``/themes/config.yml`` 中如下设置即可：

```yml
# google analytics ID
google_analytics: UA-*********-*
```

# 结语

至此网站所有的基本功能都已经实现了，以后对网站进行调整或者升级之后，会陆续将对应的修改整理更新。





