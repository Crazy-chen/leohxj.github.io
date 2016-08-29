title: jQuery选择器
date: 2013-10-14 11:19:41
categories: [JavaScript]
tags: [jQuery]
---

为什么要使用jQuery的选择器？因为jQuery选择器兼容性好，且方便绑定事件时选择元素。
<!--more-->

## 几种选择器的介绍

### Index Selectors
index selectors选择器方式按照文档中的顺序选择，比如`first-child()`,`last-child()`。常用的还有`:eq()`, `:lt()`, `:gt()`, ':event()`和`'"odd()`。

需要注意的是这里的次序是从0开始。

### Visibility Selectors
根据元素是否显示来选择元素。使用方式是: `:hidden`和`:visibile`。

依据的CSS属性是`display`或者`width`和`height`为0的情况，不包含`visibility:hidden`。

### Element-Containing Selectors
可以选择包含指定元素的元素，比如`section:has(h2)`.

### Text Containing Selectors
可以选择包含指定文本内容的元素，比如`div:contains('World')`.

### Animated Selector
jQuery有一些动画函数，比如`.slideToggle()`等。使用了这些方法的元素可以通过`:animated`选取。

## jQuery遍历的方式
上面的都是CSS选择器方式，缺点我觉得就是选择内容每次都得写死，不够灵活。

相反，jQuery提供的遍历/查找的方法，相对来说就灵活一些，并且效率高。常用的方法有:

- find
- next
- prev
- sibilings
- closest
- parent

## 参考资料 ##
[原文链接][1]
[find VS css select测试][2]


[1]: http://www.hongkiat.com/blog/jquery-selectors/
[2]: http://jsperf.com/find-select-speed-test/3