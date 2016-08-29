title: RequireJS-使用
date: 2014-04-17 14:45:41
categories: [JavaScript]
tags: [JavaScript, 模块化]
---

前面介绍过了模块化的概念，那么就说说AMD的具体实现--RequireJS。
<!--more-->

## 为什么使用RequireJS
RequireJS的提出就是为了在浏览器端更好的管理JS文件，使之模块化。主要解决的问题有两点：

1. 实现js文件的异步加载，避免网页失去响应；
2. 管理模块之间的依赖性，便于代码的编写和维护。

## 引入RequireJS
从官网下载好`require.js`，我们按照官网提供的结构，建立一下目录：
```
project-directory/
    project.html
    scripts/
        main.js
        require.js
        helper/
            util.js
```

HTML文件如下：
```
<!DOCTYPE html>
<html>
    <head>
        <title>My Sample Project</title>
        <!-- data-main attribute tells require.js to load
             scripts/main.js after require.js loads. -->
        <script data-main="scripts/main" src="scripts/require.js"></script>
    </head>
    <body>
        <h1>My Sample Project</h1>
    </body>
</html>
```
此时的`script`标签可以放在`head`中，也可以放在`body`尾部。通过`data-main`指定的文件，就已经通过异步去操作了。

main.js当作主文件, 需要依赖util文件：
```
require(["helper/util"], function(util) {
    //This function is called when scripts/helper/util.js is loaded.
    //If util.js calls define(), then this function is not fired until
    //util's dependencies have loaded, and the util argument will hold
    //the module value for "helper/util".
    console.log("helper/util", util);
});
```

util文件定义一个模块:
```
define(function() {　　　　
    var add = function(x, y) {　　　　　　
        return x + y;　　　　
    };　　　　
    return {　　　　　　
        add: add　　　　
    };　　
});
```

打开`project.html`文件，控制台会输出:
```
helper/util 
Object {add: function}
```

这样，算是完成了对requireJS的初次体验。

## Module编写
CommonJS规范推荐所有文件都是模块，AMD使用`define`定义一个模块：
```
// 没有依赖，不做处理，直接返回对象
define({
    color: "black",
    size: "unisize"
});

// 没有依赖，但是做一些处理，返回对象
define(function () {
    //Do setup work here

    return {
        color: "black",
        size: "unisize"
    }
});

// 有依赖，做处理，返回对象
define(["./cart", "./inventory"], function(cart, inventory) {
        //return an object to define the "my/shirt" module.
        return {
            color: "blue",
            size: "large",
            addToCart: function() {
                inventory.decrement(this);
                cart.add(this);
            }
        }
    }
);

// 返回函数
define(["my/cart", "my/inventory"],
    function(cart, inventory) {
        //return a function to define "foo/title".
        //It gets or sets the window title.
        return function(title) {
            return title ? (window.title = title) :
                   inventory.storeName + ' ' + cart.name;
        }
    }
);
```

使用的时候通过`require`引入，然后定义一个名称，这个名称就是返回的对象的引用。

## RequireJS的config
requirejs的配置文件可以写在html文件中，也可以写在`data-main`指定的文件中。但是小心写在`data-main`中，它的执行时异步的，不一定比其他文件先执行。基本写法如下：
```
require.config({
    baseUrl: "/another/path",
    paths: {
        "some": "some/v1.0"
    },
    waitSeconds: 15
  });
  require( ["some/module", "my/module", "a.js", "b.js"],
    function(someModule,    myModule) {
        //This function will be called when all the dependencies
        //listed above are loaded. Note that this function could
        //be called before the page is loaded.
        //This callback is optional.
    }
  );
```