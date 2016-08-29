title: hexo介绍之三（配置篇）
date: 2013-9-8 15:00:07
categories: hexo相关
tags: [hexo]
---

前两节介绍了如何搭建和书写blog。这一节介绍一些hexo的配置。<!--more-->

## 配置说明 ##
### Site信息 ###
- title: 博客的标题
- subtitle: 博客的子标题
- description: meta信息，生成在`<meta name="description" content="">`中
- author: 页脚信息，同时也生成在`<meta name="author" content="">`中
- email: 联系方式
- language: 博客显示的语言，我会设置为`zh-CN`

### URL信息 ###
如果你的blog放在二级目录下，那么同时设置下`url`和`root`。一般情况只需要设置`url`。

### 其他信息 ###
默认即可。

## 插件安装 ##
通过 `npm install xxx --save`方式安装。这里推荐两个插件：

- hexo-generator-sitemap
- hexo-generator-feed

安装完成之后，在`_config.yml`末尾处中添加：

```
# Plugins
plugins:
- hexo-generator-feed
- hexo-generator-sitemap
```
