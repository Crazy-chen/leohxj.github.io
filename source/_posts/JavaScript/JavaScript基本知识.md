title: JavaScript基本知识
date: 2013-8-2 14:45:40
categories: [JavaScript]
tags: [JavaScript]
---
JavaScript基本知识还是需要整理一下的。
<!--more-->

## 数据类型 ##
### 基本数据类型 ###
JavaScript的基本数据类型有六种：
- null
- undefined
- number
- string
- boolean
- object

### 复合类型 ###
- Object
- Array
- Date
- RegExp
- Function
- Boolean
- Number
- String

注意下，Function属于JS里面最高级别的内容，所以使用typeof返回的时候会返回`function`字符串。

### 测试类型 ###
`typeof`会返回六种字符串。比如：

```
alert(typeof 1);                // 返回字符串"number"  
alert(typeof "1");              // 返回字符串"string"  
alert(typeof true);             // 返回字符串"boolean"  
alert(typeof {});               // 返回字符串"object"  
alert(typeof []);               // 返回字符串"object "  
alert(typeof function(){}); // 返回字符串"function"  
alert(typeof null);             // 返回字符串"object"  
alert(typeof undefined);        // 返回字符串"undefined" 
```
值得注意的是，null,{},[]返回的是`object`。

`instanceof`操作符针对的是复合类型，使用方法如下：

```
var obj = new Object(); 
obj instanceof Object // 返回 true
var arr = new Array();
arr instanceof Object; // 返回 true
arr instanceof Array(); // 返回 true
```

这里需要注意的是，所有复合类型返回都是Object的对象，所以`instanceof Object`都会返回true。

### 类型之间相互转化 ###
未完待续...

## 参考资料 ##
- [JavaScript标准教程（阮一峰著）](http://javascript.ruanyifeng.com/)