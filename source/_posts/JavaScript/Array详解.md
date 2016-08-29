title: Array详解
date: 2013-10-29 11:35:21
categories: [JavaScript]
tags: [JavaScript]
---

Array应该是JS中最常用的类型了。它与其他语言中的数组还是有区别的，都是有序数组，但是每一项都可以保存任何类型的数据。
<!--more-->

## 构造方法
- 构造函数
    var arr = new Array();
- 字面赋值
    var arr = [1,2,3];

通常采用这两种方式。字面赋值的时候，注意元素末尾不要有多余的`,`号。会在IE中造成bug。

## 长度问题
获取数组的长度是`.length`属性，获取的是数组中元素的个数。但是数组访问是可以超过这个length的。获取的元素是`undefined`。