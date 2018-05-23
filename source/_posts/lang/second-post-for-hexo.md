---
title: 从零开始搭建Github Page -- 源码备份&双语网站
date: 2018-05-21 17:44:47
tags: [Web, Github Page]
categories: Github page
---
在初步架设了个人Github page后，为了方便多端同步工作，可以将源码通过Github进行备份；其次，为了适应海外工作，架设双语网站。
<!-- more -->

# 前言

在成功部署网页后，Github上只有生成后的文件，并没有网站的源文件。因此无法异地同步工作，鉴于我目前mac和window同时使用，因此有必要使用Github备份源码。其次，考虑到目前人在北美，也有双语网站的需求。

# 源码备份

备份源码最方便的方法是在``username.github.io`` 仓库（repository）下新开一个分支（branch），并使用该branch备份网站源码。

## 准备工作

首先需要在原仓库下新建一个分支。  
打开Terminal，定位到源文件所在的文件夹。

```bash
$ git remote add origin https://github.com/<username>/<username>.github.io.git # 添加远程仓库，注意最后的.git
$ git branch # 查看本地分支
$ git branch -r #
$ git branch source #
$ git checkout source #
# 
$ git checkout -b source
```

然后打开Github将repository的默认branch设为source。

### 网站主题

Github上有相当多的主题可供我们使用，比价广泛使用的是Next，本网站也是用的Next。  
载入主题只需要在``/theme``文件夹内使用``git clone``命令即可。

相关的主题可以在[Hexo themes](https://hexo.io/themes/index.html)中获取。

然后为了能同步主题配置，需要把``/theme/next``文件夹下 **.git**文件删掉。

## 备份

```bash
$ git add . # 添加所有文件
$ git commit -m 'new commit' # commit
$ git push origin source # 将修改推至云
```

## 同步

```bash
$ git pull origin source # 同步文件
```

为了实现远程同步，每次本地修改内容前，都需要将修改pull到本地。完成修改后，都需要将修改push至Giuhub。

# 网站双语

Hexo本身具有在主题配置文件``/config.yml``修改网站语言的功能，但该功能不能实现两种语言在网站上动态切换。但是我们可以利用该特性实现我们所需要的效果。

## 实现

双语网站的实现，实际上是在网页内部署了另一个完整的网站，再将其语言设置为英文。

1. 首先，我们在根目录``/``下新建文件夹``/en``，作为英文网站的根目录。
2. 第二步，在``/en``文件夹下初始化网站，或将原文件复制一遍（注意删除 **.git** ）。
3. 修改``/en``文件夹下的配置文件。

### 修改配置文件

#### 中文页面

中文网站中，一共有3处需要修改。

1. 站点配置文件

    ```yml
    # site
    language: zh-Hans
    ```

2. 主题配置文件

    ```yml
    # Menu settings
    menu:
        commonweal: /en
    ```

3. 主题目录下的语言文件``/themes/next/language/zh-Hans.yml``

    ```yml
    menu:
        commonweal: EN
    ```

#### 英文网站

英文网站中，同样有3处需要修改。

1. 站点配置文件

    ```yml
    # site
    language : en

    # URL
    root: /en/

    # Directory
    public_dir: ../public/en

    # Writing
    new_post_name: lang/:title.md
    ```
2. 主题配置文件

    ```yml
    # Menu Settings
    menu:
        commonweal: ../
    ```

3. 语言文件（``/en/themes/languages/en.yml``)

    ```yml
    menu:
        commonweal: 中文
    ```

## 部署

由于/en在原网站生成的过程中不会自动生成，因此实际部署双语网站需要先分别在两个网站中执行生成命令，再统一部署。