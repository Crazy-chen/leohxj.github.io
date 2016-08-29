title: nodejs开发命名行工具
date: 2014-07-02 14:31:46
categories: [Nodejs]
tags: [nodejs, commander]
---

受启发的缘由一个是[nodecup](https://github.com/Edmeral/nodecup)项目，可以命令行形式查看世界杯赛程。另一个是[commander](https://visionmedia.github.io/commander.js/), 方便编写命令行工具的库。
 
结合起来，我觉得对以后的自动化部署会有帮助，所以先尝试去写一个获取天气的命令行工具。
<!--more-->

## 项目分析
我想要做的是通过命名行的方式，直接查询当前时刻的天气，以及最近几天的天气情况。命名行下我预测会出现的问题就是获取当前位置，所以默认情况还是需要手动输入一个城市名称。
 
项目我放在了github上，使用的是Node.js完成。项目地址:[nodeweather](https://github.com/leohxj/nodeweather)。
 
## 天气API
关于天气的API，知乎上有一个讨论[网上的天气 API 哪一个更加可靠？](http://www.zhihu.com/question/20575288)。
 
我相信中国天气的API比较官方，但是个人使用需要申请验证，[验证方式](http://smart.weather.com.cn/wzfw/smart/weatherapi.shtml)。使用过程中，也是需要根据城市查询到对应的城市代码才可以查询，不是很方便，因为网上没有直接查询这些城市代码的接口。我可能需要自行建立数据库。
 
接下来我想到了Yahoo Weather， 目前很多天气源也是引用的雅虎数据。它的在线手册是:[yahoo weather](https://developer.yahoo.com/weather/#response)。使用雅虎天气的API，需要有一个`woeid`，这个正常情况下需要申请yahoo的Developer API。但是幸运的是我发现一个地址可以通过城市名称查询这个`woeid`，在线地址`http://query.yahooapis.com/v1/public/yql?q=select*from%20geo.places%20where%20text=%22%22&format=xml`，传入的城市名称设置在`text=%22%22`两个`%22`中。返回的是XML格式数据。里面会有响应的`woeid`。
 
获取到了`woeid`之后，我们可以通过雅虎天气的API查询天气了，查询方式如下：`http://weather.yahooapis.com/forecastrss?w=615702&u=c`。参数`woeid`就是城市对应的代码，`u=c`代表查询的是摄氏温度。当然，返回的对象依旧是XML格式。
 
## 处理XML
因为是在服务器端处理，并且使用的是node.js环境。我还是习惯性的使用`jQuery`去解析XML。
 
在nodejs中使用jQuery，会有一个问题就是缺少window变量。需要引入`jsdom`帮助创建`window`对象。
 
官方的使用方式如下:
```
In Node.JS you may also create separate window instances
 
    var jsdom = require('jsdom').jsdom
      , myWindow = jsdom().createWindow()
      , $ = require('jQuery')
      , jq = require('jQuery').create()
      , jQuery = require('jQuery').create(myWindow)
      ;
 
    $("<h1>test passes</h1>").appendTo("body");
    console.log($("body").html());
 
    jq("<h2>other test passes</h2>").appendTo("body");
    console.log(jq("body").html());
 
    jQuery("<h3>third test passes</h3>").appendTo("body");
    console.log(jQuery("body").html());
```
 
我个人创建window使用的方式如下:
```
var jsdom = require("jsdom");
var $ = require("jquery")(jsdom.jsdom().createWindow());
```
 
nodejs处理网络请求使用的模块是`request`, 具体使用参考文档即可。
 
## 脚本的编写
这里如要提及一下脚本的编写问题，我是在windows下写的程序，但是为了保证程序在unix下正常工作，我需要在文件头部声明好系统的环境变量，nodejs写的脚本应该为:
 
```
#!usr/bin/env node
```
 
sublimetext编辑器需要注意设置`view->line-coding`为`unix`模式，如果模式是`windows`，在unix下，换行会多出`/r`字符，报错信息大致为`ERROE env: node/r`。
 
本地测试的使用，你使用的是`node xxx`形式，只要你在`package.json`中指定了:
```
"main": "./nodeweather",
  "bin": {
    "nodeweather": "./nodeweather.js"
  }
```
上传至npm之后，再下载就可以直接使用xxx形式。


## 上传NPM
首先，你需要拥有一个npm的账号，然后正确的设置好`package.json`。
 
使用`npm adduser`进行验证，使用`npm publish`上传文件至npm。
 
完成之后，就可以在任意环境下使用`npm install xxx`安装了。
 
## 不借助commander的情况
有时候我们只是写一段脚本，想要在全局运行，那么也很简单。

首先确保项目中存在`package.json`文件，如果没有，自行`npm init`一个，比如:

```
{
  "name": "sayhi",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "bin": {
    "hw": "index.js"
  },
  "author": "",
  "license": "ISC"
}
```

其中特别要注意的是:

```
"bin": {
  "hw": "index.js"
}
```
这段，标明了希望使用`hw`达到`node index.js`的目的。

其次我们要在脚本文件的开头，加上:

```
#!usr/bin/env node
```

### 全局安装
使用`npm install -g`或者`npm link`即可。
然后全局下就可以使用了。

这里区分一下`npm install -g`与`npm link`的区别：
- `npm install -g`安装的相当于硬链接，只保留当前版本，本地修改项目不会同步到命令中。
- `npm link`其实就是一个软连接，本地的任何修改都能同步到命令之中。

### 撤销全局安装
`npm uninstall -g <name>`


## 参考资料
- [Yahoo Weather API](https://developer.yahoo.com/weather)
- [获取城市WOEID代码方式](http://android-er.blogspot.jp/2012/03/search-woeid-from-httpqueryyahooapiscom.html)
- [nodecup](https://github.com/Edmeral/nodecup)
- [commander](https://visionmedia.github.io/commander.js/)