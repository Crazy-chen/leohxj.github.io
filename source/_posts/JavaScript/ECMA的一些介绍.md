title: ECMAScript的一些介绍
date: 2016-09-06 10:00:00
categories: [JavaScript]
tags: [JavaScript]
---

学习 JavaScript 的过程中，你肯定听说过 ECMAScript 。但是你知道它到底是什么吗?
<!--more-->

## JavaScript 介绍
它是一种解释型的编程语言，由 `ECMA` 通过 `ECMAScript` （又称为 `ECMA-262`）实现语言的标准化。`JavaScript` 是被主流浏览器支持的。它的 [实现](http://www.w3school.com.cn/js/pro_js_implement.asp) 应该包含三个部分:

- ECMAScript: 语言核心，操作基本语法和对象，比如 `Array` 数组操作。
- DOM: 文档对象模型，操作网页内容的方法和接口, 比如`document`, `event`, `document.getElementById` 等等。
- BOM: 浏览器对象模型， 操作与浏览器进行交互的方法和接口，比如 `window` , `history`, `location` 等等。

关于 `DOM` 和 `BOM` 的区别：

- `DOM` 操作的是网页内的东西，`BOM` 操作的是浏览器相关内容。
- `DOM` 是 `W3C` 的标准， 而 `BOM` 是各个浏览器厂商根据 `DOM` 在各自浏览器上的实现。
- `document` 对象是 `DOM` 的规范实现，也是 `BOM` 的对象。

还有一个常见的问题：**ECMAScript和JavaScript到底是什么关系？**

要讲清楚这个问题，需要回顾历史。1996年11月，JavaScript 的创造者 Netscape 公司，决定将 JavaScript 提交给国际标准化组织 ECMA，希望这种语言能够成为国际标准。次年，ECMA 发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript，这个版本就是1.0版。

该标准从一开始就是针对 JavaScript 语言制定的，但是之所以不叫 JavaScript，有两个原因。一是商标，Java是Sun公司的商标，根据授权协议，只有Netscape公司可以合法地使用 JavaScript 这个名字，且JavaScript 本身也已经被 Netscape 公司注册为商标。二是想体现这门语言的制定者是 ECMA，不是 Netscape，这样有利于保证这门语言的开放性和中立性。

因此，ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 扩展还有 Jscript 和 ActionScript）。JavaScript 存在的意义主要是为了解决在客户端对网页进行操作。

## ECMA 组织
以前是叫 `European Computer Manufacturers Association`, 1994 年改名为 `Ecma International`, 是一家国际性会员制度的信息和电信标准组织。官网为 [ecma-international](http://www.ecma-international.org/)。相关的标准都可以从中查询到，我们一般关注的就是 [ECMA-262 标准](http://www.ecma-international.org/publications/standards/Ecma-262-arch.htm)。

## ECMAScript
ECMAScript 是由 ECMA-262 标准化的脚本语言的名称, JavaScript 是它的一种实现， 其他的实现还有 Falsh 中的 ActionScript。ECMA-262 标准（第 2 段）的描述如下：

> “ECMAScript 可以为不同种类的宿主环境提供核心的脚本编程能力，因此核心的脚本语言是与任何特定的宿主环境分开进行规定的... ...”

ECMAScript 仅仅是一个描述，定义了脚本语言的所有属性、方法和对象。其他语言可以实现 ECMAScript 来作为功能的基准，JavaScript 就是这样, ECMAScript 是它的核心。

## TC39
标准需要有人制定， TC39 就是一组开发 ECMA-262 标准规范的人。 它是 `Ecma第39号技术委员会` 的意思。

## ECMAScript 版本
我们常见的 `ES3`, `ES5`, `ES6` 就是 ECMAScript 的版本。它对应的也是 `ECMA-262` 的版本。它的历史大概如下:

- 1996年11月，网景公司将 JavaScript 提交给 ECMA 进行标准化。
- 1997年6月，ECMAScript 1.0版发布。
- 1998年6月，ECMAScript 2.0版发布。
- 1999年12月，ECMAScript 3.0版发布，成为JavaScript的通行标准，得到了广泛支持。
- 2007年10月，ECMAScript 4.0版草案发布，对3.0版做了大幅升级，预计次年8月发布正式版本。草案发布后，由于4.0版的目标过于激进，各方对于是否通过这个标准，发生了严重分歧。以Yahoo、Microsoft、Google为首的大公司，反对JavaScript的大幅升级，主张小幅改动；以JavaScript创造者Brendan Eich为首的Mozilla公司，则坚持当前的草案。
- 2008年7月，由于对于下一个版本应该包括哪些功能，各方分歧太大，争论过于激进，ECMA开会决定，中止ECMAScript 4.0的开发，将其中涉及现有功能改善的一小部分，发布为ECMAScript 3.1，而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该版本的项目代号起名为Harmony（和谐）。会后不久，ECMAScript 3.1就改名为ECMAScript 5。
- 2009年12月，ECMAScript 5.0版正式发布。Harmony项目则一分为二，一些较为可行的设想定名为JavaScript.next继续开发，后来演变成ECMAScript 6；一些不是很成熟的设想，则被视为JavaScript.next.next，在更远的将来再考虑推出。
- 2011年6月，ECMAscript 5.1版发布，并且成为ISO国际标准（ISO/IEC 16262:2011）。
- 2013年3月，ECMAScript 6草案冻结，不再添加新功能。新的功能设想将被放到ECMAScript 7。
- 2013年12月，ECMAScript 6草案发布。然后是12个月的讨论期，听取各方反馈。
- 2015年6月17日，ECMAScript 6发布正式版本，即ECMAScript 2015。

## 浏览器对 ECMAScript 的支持
浏览器是我们执行 JavaScript 的宿主环境之一，指定的标准需要宿主环境提供支持。目前所有浏览器都遵守 ECMA-262 第三版。需要注意的是 IE 浏览器， IE9及其以上版本才提供对 ES5 的支持。 所以在 IE6~8 下，如果我们需要使用 ES5 的一些方法，需要自行添加 `polyfill` 。同样的，目前大部分主流浏览器 (Chrome, Safari, firefox)，是支持到 ES5 的，如果我们需要使用 ES6 标准，需要借助语法转换工具，比如 [Babel](https://babeljs.io/), 对我们的代码进行转码，变成 ES5 支持的代码，然后在浏览器中运行。

关于 ECMAScript 浏览器兼容性，可以查询： [ECMAScript compat-table](http://kangax.github.io/compat-table/es5/)。


## 参考
- [JavaScript 实现](http://www.w3school.com.cn/js/pro_js_implement.asp)
- [JavaScript Vs DOM Vs BOM, relationship explained](https://vkanakaraj.wordpress.com/2009/12/18/javascript-vs-dom-vs-bom-relationship-explained/)
- [ECMA-262 标准](http://www.ecma-international.org/publications/standards/Ecma-262-arch.htm)
- [读懂 ECMAScript 规格](http://www.ruanyifeng.com/blog/2015/11/ecmascript-specification.html)
- [ECMAScript各版本简介及特性](http://www.07net01.com/2015/08/913846.html)
- [《献给你，我深爱的ECMAScript》开篇](http://www.w3cplus.com/js/ecmascript-lesson-1.html)
