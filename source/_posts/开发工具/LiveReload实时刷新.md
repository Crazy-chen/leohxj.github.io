title: LiveReload实时刷新
date: 2013-10-8 14:09:58
categories: [开发工具]
tags: [前端工具]
---

前端开发离不开HTML, CSS, JS这三样。现在很多工具提供了预编译的操作，比如LESS, CoffeeScript，使用他们的过程之中必然经过修改，保存，编译，刷新预览这样的操作才可以看到效果。

这么机械的操作当然是可以简化的， 那就是LiveReload。<!--more-->

## 什么是LiveReload
简单的说，LiveReload就是实现了浏览器实时刷新，文件一旦修改，立刻体现在浏览器上。它的实现有很多插件，比如ruby上的，sublime上的。我推荐使用官方的客户端，它既可以结合浏览器实现实时刷新，还可以即时编译文件。

不推荐sublime的插件的原因是，它只能检测当前编辑的文件，如果你是LESS文件，编译之后不能体现在浏览器中。

## 如何使用
两个必备条件: 客户端软件和浏览器插件。

客户端软件会在本地开启一个监听端口，浏览器插件需要开启本地文件访问的权限。这样文件修改之后就可以在浏览器中自动刷新了。

## 结合LESS/CoffeeScript
如果你书写的是这些预编译的代码，那么你需要使用grunt或者Koala这样的工具进行一次转换。虽然LiveReload自带了编译功能，但是目前来看功能还是比较单一。我比较推荐自定义更强的grunt或者客户端koala.

## 我的用法
目前我使用LiveReload客户端开启监听端口，Chrome浏览器的LiveReload插件开启监听功能。简单的文件编译我就是用LiveReload自带的功能。如果遇到需要拼接，压缩的，我会使用Koala或者grunt来完成。

## 参考资料
- [livereload](http://livereload.com/)
- [brower extension](http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-)
- [grunt](http://gruntjs.com/)
- [Koala](http://koala-app.com/)