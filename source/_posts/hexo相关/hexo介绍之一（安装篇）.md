title: hexo介绍（安装篇）
date: 2013-9-7 13:00:07
categories: hexo相关
tags: [hexo]
---

前几天hexo升级，直接导致我本地生成文章出错。询问了tommy351，原来是主题更新了。换了新的主题之后，一些设置又不可用了，一怒之下，我决定记录一下hexo的使用。<!--more-->

## 安装hexo ##
首先你的机器安装了nodejs,通过npm就可以安装hexo了。命令如下：

```
npm install -g hexo
```

更加详细的安装说明，请参考[官方安装说明](http://zespia.tw/hexo/docs/)。

## 建立Blog ##
通过`hexo init <目录名称>`来快速的建立一个blog目录。结构如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

需要说明的是：

- _config.yml: blog的配置文件
- source/_posts: 存放要发布的文章
- themes: 博客主题

其余文件无需使用。

## 查看blog ##
默认在`_posts`中有一篇helloworld.md文章，所以直接使用命令：

```
hexo generate

hexo server
```
即可在`localhost:4000`下访问你的blog了。

