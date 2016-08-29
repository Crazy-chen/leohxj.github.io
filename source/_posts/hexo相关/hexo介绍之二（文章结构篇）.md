title: hexo介绍之二（文章结构篇）
date: 2013-9-8 13:00:07
categories: hexo相关
tags: [hexo]
---

既然第一步blog都搭建好了，下面就开始写文章吧！<!--more-->

## 文章结构 ##
文章当然不是随便乱写的，也是要有结构的，它是markdown文档（一种标记行语言）。但是为了方便解析，又遵循了Yaml文档格式。所以嘛，还是有必要说说的。基本结构如下：

```
title: hexo介绍之二（文章结构篇）
date: 2013-9-8 15:47:52
categories: 博客相关
tags: [hexo]
---

既然第一步blog都搭建好了，下面就开始写文章吧！<!--more-->
```

## 标签说明 ##
- title: 文章显示的标题
- data: 文章创建的时间
- categories: 文章的归类
- tags: 文章的标签，多个标签使用`[tag1, tag2, tag3]`这样的形式
- <!--more-->: Learn More分隔符

以上四个标签是我常用的，全部内容可以参考:[hexo书写介绍](http://zespia.tw/hexo/docs/writing.html)。在`---`之后，开始你的文章，注意书写使用markdown格式。推荐标题从h2开始（因为h1已经是title了）。
