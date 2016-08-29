title: GSAP介绍
date: 2013-7-27 16:39:37
categories: [UED]
tags: [GSAP, 动画]
---
在FLASH鼎鼎大名的`GreenSock`也是有JS版本的，据说它在AS界的地位好比JS中的`jQuery`。所以，有必要好好的了解一下这个强大的动画引擎。给出一篇InfoQ上的介绍[GreenSock推出了新一代动画引擎平台GSAP v12](http://www.infoq.com/cn/news/2012/05/gsap-v12)。
<!--more-->

# 简单介绍 #
性能强悍，自称效率是jQuery的20倍。因为它利用CSS渲染，不会影响JS的处理。具体的介绍，官网上一篇文章写的很好：(Why GSAP for HTML5 Animation?)[http://www.greensock.com/why-gsap/]。我就不多说了。

# 使用 #
GSAP里包含四大部分，分别是`TweenLite`, `TweenMax`, `TimelineLite`, `TimelineMax`。`TweenMax`是包含了其他所有东西的文件，所有如果不介意文件大小，一般直接使用`TweenMax`。


## TweenLite ##


# 缓动 #
我觉得缓动是动画比较主要的一部分，一个元素平淡的移动体现不出什么效果，加上了缓动之后，效果就不一样了。关于GSAP里的缓动，可以参考[缓动列表](http://www.greensock.com/roughease/)。貌似JS版本不支持自定义缓动，算是一个遗憾（或者是我不知道。。。）。

# 相关资料 #
- [Getting Start](http://www.greensock.com/get-started-js/)
- [GSAP文档](http://api.greensock.com/js/)
- [缓动列表](http://www.greensock.com/roughease/)
