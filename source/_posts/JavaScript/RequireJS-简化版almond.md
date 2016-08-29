title: RequireJS-简化版almond
date: 2014-04-20 14:45:41
categories: [JavaScript]
tags: [JavaScript, 模块化]
---

almond是requireJS作者的另一个简化版AMD加载项目，它的特点是在build之后，网络请求只留一个，而requireJS的请求有两个（另一个是requireJS本身）。并且自身只有1kb左右的大小(服务器开启gzip后)。
<!--more-->

## 使用almond
开发过程还是使用requireJS，只是build的时候，去掉requireJS而加入almond而已。能够减少requireJS的多余大小和一次请求。

使用方式通过`r.js`，`almond-build.js`文件如下：
```
({
    baseUrl: ".",
    paths: {
        jquery: "some/other/jquery"
    },
    name: "./almond",
    include: "main",
    out: "main-built.js",
    wrap: true
})
```
保证`r.js`和`almond-build.js`在`script`目录下，运行`node r.js -o almond-build.js`即可。生成的`main-built.js`文件，可以直接通过`script`引入到index.html中。

## 参考资料
- [almond项目地址](https://github.com/jrburke/almond)