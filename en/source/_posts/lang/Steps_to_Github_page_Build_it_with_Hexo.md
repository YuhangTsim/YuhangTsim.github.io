---
title: Hexo入门 Steps to Github Page -- Intro of Hexo
date: 2018-05-15 12:54:51
tags: [Web, Github Page]
---
Start from the beginning, build a personal Github Page by Hexo.
<!-- more -->

# Intro

Google is always the first step to do somthing and no exception for building a Github page. There are lots of tutorials in the web for how to build up such a blog, you can also search on Youtube for vedio tutorial. But somehow I think English tutorial is more like a basic knowledage, they build their pages with Github build-in theme. There are too much for me to set up at the beginning if I do that. Then I turned to Chinese tutorial and found out this Hexo.

There are lots of platforms for someone to build their blog, for example, Wordpress, CSDN, or setup a personal sever. The reason I go for Github is that this is the only way that I can own the website totally beside building a sever by myself which will bring too much technical stuff and doesn't fit my original intention to mark down and systemize the knowledge I learned.

Although there are tons of tutorials you can easily reach, the most importance thing I want to emphasize is that you need to keep thinking, analyzing, and logicing down everytime you face a problem. Because these tutorials can not meet all your requirement.

There are lots of reference in the whole procedure, here are some important ones (In Chinese):

1. [Blog Build a free statis Blog with Github and Hexo](https://wsgzao.github.io/post/hexo-guide/)
2. [Hexo document](https://hexo.io/zh-cn/docs/)
3. [Add Search on your Hexo blog](http://www.itfanr.cc/2017/10/27/add-search-function-to-hexo-blog/)
4. [Build a dual-language blog on hexo](https://chenyxmr.github.io/2016/08/04/hexo-bilingual/)  

## Preparation

This tutorial focuses on <font color=#6e081d>``Mac``</font> and <font color=#6e081d>``Win``</font>，based on <font color=#6e081d>``Hexo 3.7``</font>。

#### About Hexo

[Hexo.io](hexo.io):
Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.

#### Install Git, Node.JS, Hexo

Follor the official document to install [Git](https://git-scm.com/)， [Node.js](https://nodejs.org/).

```bash
# To check the installation
$ npm -v # 5.6.0
$ node -v # v8.11.1
$ git --version # git version 2.16.3
```

##### Hexo

First step, installation

```bash
#  Install Hexo
$ npm install -g hexo-cli
```

There are two ways to initialize the website.

```bash
# 1. Initialize in target folder
$ hexo init 
# 2. Initialize in new folder
$ hexo init <folder>

$ cd folder
$ npm install
# Success, if there is no ouput
```

Install extensions.

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

That is all for basic installation.

###### Deploy to local

```bash
$ hexo server
# Go localhost:4000 to see the page
```

###### Basic command for Hexo

```bash
$ hexo n 'post_name' # 新建post页面 (new) Create a new post
$ hexo new page "page_name" # 新建网络页面 (new page) Create a new post
$ hexo g # 生成网页 (generate) Generate the website
$ hexo s # 启动服务器 (server) Start local sever
$ hexo d # 部署网站 (deploy) Deploy the website
$ hexo clean # 清除缓存文件 Clean cache
```

## Configeration

### Github
Github provides a free domain, username.github.io, for every account. We can use this as the domain of our blog or buy a domain to replace it.

To use the github page, we need to create a new repository called **\<username>.github.io**.

And we also need to confige SSH and the repository.
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

## 最后

部署网页的内容在以上应该都覆盖到了，接下来的的文章依次是

1. Github备份网页源码以及双语网页设置
2. 网站页面、内容、功能的添加
3. ...