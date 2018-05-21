---
title: Steps to Github page-Build it with Hexo
date: 2018-05-15 12:54:51
tags:[Web, Github Page]
---

## 前言
从零开开始一个博客，只能从Google开始。网上能轻易搜到的教程不少，但用的平台无非几个，CSDN, 简书，知乎专栏，Github，Wordpress。对于我而言，前面的三个都不是我的选择，因为他们都没办法自己管理数据，所以只能选择Github page或者Wordpress。本来wordpress已经几乎搭建好了，但是域名转接要花钱，觉得暂时没必要，故而选择Github。Github自由一点，可以随意发挥，只是又一次开始了无穷的折腾。

首先，最重要的是，诸多的帖子都不一定能满足自己的需求或者达到它所描述的效果。关键还事在于不断的思考和分析，然后摸索出自己的路。  

整个过程参考过的帖子很多，这里放出来一部分比较重要的：
[使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
[Hexo document](https://hexo.io/zh-cn/docs/)
[Hexo博客添加搜索功能](http://www.itfanr.cc/2017/10/27/add-search-function-to-hexo-blog/)
[hexo实现中英双语博客教程](https://chenyxmr.github.io/2016/08/04/hexo-bilingual/)  

## 准备工作
本文针对<font color=#6e081d>``Mac``</font>和<font color=#6e081d>``Win``</font>，以及<font color=#6e081d>``Hexo 3.7``</font>。

#### About Hexo
[Hexo.io](hexo.io):
Hexo是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析    文章，在几秒内，即可利用靓丽的主题生成静态网页。

#### 安装Git, Node.JS, Hexo
[Github](https://github.com/)， [Node.js](https://nodejs.org/) 按着官网步骤安装即可。  

```bash
# 检查是否安装成功
$ npm -v # 5.6.0
$ node -v # v8.11.1
$ git --version # git version 2.16.3
```
##### Hexo
首先是安装。

```bash
# 安装Hexo
$ npm install -g hexo-cli
```

初始化网站有两种方法。

```bash
# 1. 在目标文件夹内初始化
$ hexo init # 初始化网站
# 2. 新建文件夹初始化
$ hexo init <folder>

$ cd folder
$ npm install 
# 没有任何错误提示的话，就是成功了
```

完成之后安装插件。

```bash
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```

至此初步的安装就算结束了。

###### 本地查看

```bash
$ hexo server
```
到[本地](localhost:4000)查看效果即可。

###### Hexo基本操作
```bash
$ hexo n 'post_name' # 新建post页面 (new)
$ hexo new page "page_name" # 新建网络页面 (new page)
$ hexo g # 生成网页 (generate)
$ hexo s # 启动服务器 (server)
$ hexo d # 部署网站 (deploy)
$ hexo clean # 清除缓存文件
```

## 配置
### Github
每个账号能提供一个免费的域名给我们建立Github page，username.github.io。我们可以直接使用这个域名，也可以使用自己购买的域名。

使用Github page我们需要用username.github.io作为名字新建一个repository，新建完成后使用默认配置即可。

为了正常的部署网站，还需要配置SSH和Github仓库。
具体参考[技术小白搭建个人博客 github+hexo](https://zhuanlan.zhihu.com/p/32957389)中的配置部分。
### Hexo
为了hexo的部署能完成，还需要在``./_config.yml``文件里进行配置。
##### 配置文件： _config.yml
在配置之前，首先是要了解Hexo的配置文件。配置文件一般有两种，中文叫``站点配置文件``和``主题配置文件``，分别在``./``和``./themes/themes_name/``下。
站点配置文件一般用于设置网站的基本信息，譬如**网站名**， **描述**，**网址**，**主题**，**生成路径**等，而主题配置文件一般用于设置网站的结构和功能，譬如**页面**， **主题*模版*等。

部署网页的配置在站点配置文件之下，按如下配置：

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/<User_name>/<User_name>.github.io.git
  branch: master
```
需要特别注意的是，repo一行最后的**``.git``**。

## 网页部署
在网页源文件夹下打开命令行，执行以下命令即可：

```bash
$ hexo  clean
$ hexo g
$ hexo d
# 或者
$ hexo clean
$ hexo d -g
```

然后直接在浏览器中打开[username.github.io](username.github.io)，观看网页部署的效果。一般每次部署都要等几分钟更新。

## 个人域名
如果有需要使用个人域名，在购买了自己的域名后需要对DNS 服务进行配置。
由于我在[namecheap](https://www.namecheap.com/)上买的域名，以下配置都以**namecheap**为例。
打开**Dashboard**之后，进入**AdvanceDNS**设置，删除所有默认的Host record。添加3条新的Record如下：

| Type  | Host | Value | TTL |
| ------- | ------ | ------ | ------ | 
| A Record | @ | 192.30.252.153 | Automatic |
| A Record | @ | 192.30.252.153 | Automatic |
| CNAME | www| username.github.io | Automatic |

其次，在``./source``文件下新建``CNAME``文件，内容如下，注意文件无后缀。

```
yourdomain.address
```
保存，重新部署网页即可。

# 最后
部署网页的内容在以上应该都覆盖到了，接下来的的文章依次是

1. Github备份网页源码以及双语网页设置
2. 网站页面、内容、功能的添加
3.  ...



