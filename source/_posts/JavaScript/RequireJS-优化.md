title: RequireJS-优化
date: 2014-04-19 14:45:41
categories: [JavaScript]
tags: [JavaScript, 模块化]
---

开发完成之后，部署的时候需要对requirejs进行优化。以合并文件，减少请求。
<!--more-->

## 使用r.js
可以通过官网下载`r.js`，也可以使用`npm install -g requirejs`安装。windows用户注意需要在cmd中重新定义下r.js命名： `DOSKEY r.js=r.js.cmd $*`。

### 命令行模式
保证r.js和`data-main`指定的文件在同一目录，可以在命名行下使用 `node r.js -o baseUrl=. paths.jquery=some/other/jquery name=main out=main-built.js`。

### build.js配置文件模式
也可以在配置一个`build.js`文件，比如：
```
({
    baseUrl: ".",
    paths: {
        jquery: "some/other/jquery"
    },
    name: "main",
    out: "main-built.js"
})
```
通过`node r.js -o build.js`调用生成`main-built.js`文件。

### 使用build过后的文件
把`data-main`修改为生成好的文件，这样请求就只有`require.js`和`main-built.js`两个文件了。

## 使用r.js发布整个项目
r.js也可以build整个项目，完成css的合并，js的压缩：
```
({
    appDir: "../",
    baseUrl: "scripts",
    dir: "../../appdirectory-build",
    modules: [
        {
            name: "main"
        }
    ]
})
```
将此文件存为`app-build.js`，然后运行`node r.js -o app-build.js`，就会在项目目录同级创建一个目录。


## 参考资料
- [r.js配置文件不完整中文注释](http://nomospace.github.io/posts/r.js-example.build.js.html)
- [前端优化：RequireJS Optimizer 的使用和配置方法](http://blog.segmentfault.com/f2e/1190000000394849)
- [requirejs官方优化文档](http://requirejs.org/docs/optimization.html)