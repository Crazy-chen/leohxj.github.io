title: hexo速食指南
date: 2016-8-29 16:00:07
categories: hexo相关
tags: [hexo]
---

基于 Hexo 3.x版本, 配合 github，带领大家快速构建自己的 blog。<!--more-->

之前搭建自己 blog 的时候，整理过几篇文章，一年多过去了，Hexo 也已经升级到了 3.x 版本，和之前的版本使用上稍有差别。这次更新 hexo，顺便再次记录一下博客的构建。

## 什么是 Hexo
> Hexo: A fast, simple & powerful blog framework.

官方的一句话介绍：“快速，简单，强大的博客框架”。

实质完成的工作是：让我们以`markdown`形式书写博客内容，通过 Hexo 帮助我们生成静态文件，配合 github， 或者其他静态资源服务器进行部署。

## 安装 Hexo
使用 Hexo 有两个必要工具需要确保安装：

- git
- node

使用 `npm i -g hexo-cli` 安装 `hexo` 命令行工具。

## 创建 blog 目录
`hexo init <folder>` 创建我们的 blog 目录。

此命令会帮助我们生成目录，并进入执行 `npm install`，如遇失败，我们需要手动进入生成的目录，并再次执行 `npm install`。

## 创建文章
`hexo new [layout] <title>` 命令可以帮助我们创建文章模版。

layout支持三种类型，对应生成文件在不同的目录下:

- `post`: `source/_posts`
- `page`: `source`
- `draft`: `source/_drafts`

如果不传递 `layout`， 默认生成的就是 `post` 类型。文件在 `source/_posts` 目录下， 文件的内容结构大致如下:

```
---
title: {{ title }}
date: {{ date }}
tags:
---
```

这一段相当于是文件信息， 是给 Hexo 使用，帮助我们生成目录，标签等内容的。 它下面开始的就是我们实际的文件内容，使用 `markdown` 语法编写。

## 本地预览
`hexo server` 命令帮助我们在本地实现预览。

默认开启的是 4000 端口， 通过 `localhost:4000` 即可访问。

## 生成静态文件
`hexo generate` 命令帮助我们生成静态资源文件。

生成的文件在 `public` 目录下，我们只要将这个目录内容发布到 github 或 静态资源服务器上，即可通过域名访问了。

## 部署
这里讲解配合 github 部署的方案， Hexo 3.x 版本后， 部署git需要安装一个插件: [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git), 然后设置我们的 `_config.yml` 配置文件:

```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
```

填上我们项目的url, branch 信息，即可通过 `hexo deploy` 实现发布到对应 github 仓库中了。

**注意**： github 默认提供一个 `username.github.io` 仓库， 用来给我们存放个人相关站点， 通过域名 `username.github.io` 即可访问。原来需要设置`gh-page`分支，现在也可以直接指定静态资源访问的分支了。如有不明白的地方，自行 google 一下吧， 只要达到将静态文件存放到这个仓库中，就能实现访问了。

## 主题切换
基本的 blog 系统算是完成了，我们对界面或许还有些追求， Hexo 也提供了许多主题供我们选择，在 [Hexo Themes](https://hexo.io/themes/) 可以查看。

选择了一个主题之后，我们可以将它 `clone` 或 `download` 到本地，放置在 `theme` 目录下， 然后修改 `_config.yml` 文件，配置主题名称, 比如我的这个主题 `Hacker`: 

```
theme: Hacker
```

主题相关的设置，比如是否有评论组件，google统计等设置，均在主题目录下的 `_config.yml` 中进行配置，具体的配置要查看主题文档。


## 添加插件
通常我们还会为博客生成 `sitemap` 和 `RSS` 订阅需要的文件，活着添加本地保存刷新功能。这些是需要插件完成的，下载对应的插件，然后在 `_config.yml` 中进行对应配置即可。这里推荐几个插件:

- hexo-generator-sitemap: 生成网站的sitemap文件
- hexo-generator-feed: Generate Atom 1.0 or RSS 2.0 feed.
- hexo-generator-archive: Archive generator plugin for Hexo.
- hexo-deployer-git: Git deployer plugin for Hexo.
- hexo-browsersync: 自动刷新功能

## 域名绑定
进过了以上的一些设置，我想各位应该可以搭建一个类似本站的博客了。最后一步可能就是需要设置一个自己的域名了。通过需要两步：

- 在域名系统中添加 A纪录 和 `CNAME`： 通过自己的域名，能够访问 `username.github.io`
- 在`source`目录下添加 `CNAME`： 通过`username.github.io`，能够跳转到自己的域名

`CNAME`文件内容，就是自己的域名地址。

## 写在最后
最后说一点，博客记录的是自己的产出，多花点时间专注在内容上 :)





