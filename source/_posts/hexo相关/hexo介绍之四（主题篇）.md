title: hexo介绍之四（主题篇）
date: 2013-9-8 16:00:07
categories: hexo相关
tags: [hexo]
---

默认的布局和显示内容可能不是我们想要的，所以对于主题，还需要略作修改。<!--more-->

## 导航栏 ##
想要添加一个页面模块，首先需要在`source`下建立一个目录，比如`about`。然后在`themes/light/_config.yml`中进行设置：

```
menu:
  首页: /
  归档: /archives
  关于我: /about
  RSS: /atom.xml
```
