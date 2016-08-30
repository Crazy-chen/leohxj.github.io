title: 实战运用 Babel
date: 2016-08-30 10:00:00
categories: [JavaScript]
tags: [JavaScript, ES6]
---
babel 是一个转码工具，可以将使用 ES6 语法编写的代码，转换为 ES5 代码，从而在现有的环境中运行。
<!--more-->

我们运行的场景大概有分为 demo性质的小练习 与 实际的工程项目。

## `.babelrc` 文件
使用 Babel 的时候，基本上我们都要配置 `.babelrc` 文件，用来指定使用的 `preset` 和 `plugin`。比如:

```
{
    "preset": ["es2015"]
}
```

## Read-Eval-Print-Loop (REPL)
如果你想立即尝试一下ES6，那么Babel这个在线的交互式运行环境最合适: [Babel REPL](http://babeljs.io/repl/)。

比如你输入:
```javascript
{
  let a = 10;
}
console.log(a);
```
就会立刻转码为:
```javascript
"use strict";

{
  var _a = 10;
}
console.log(a);
```
并且在右下方有输出模式。


## 命令行
在线运行可以使得我们尝试一下ES6的语法，但是当我们想要在本地运行的时候，就需要借助Babel-cli或babel-node（包含在babel-cli中）了。全局安装，`npm install -g babel-cli`。如果只是想针对某个项目安装的话，使用:`npm install --save-dev babel-cli`或者在`package.json`中，添加:
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "devDependencies": {
    "babel-cli": "^6.0.0"
  }
}
```

**尽管你可以把 Babel CLI 全局安装在你的机器上，但是按项目逐个安装在本地会更好。**有两个主要的原因:
- 在同一台机器上的不同项目或许会依赖不同版本的 Babel 并允许你有选择的更新。
- 这意味着你对工作环境没有隐式依赖，这让你的项目有很好的可移植性并且易于安装。

安装在本地的话，运行需要指定`node_modules`下面的模块，即`node_modules\.bin\babel --help`方式。但还有一个快捷方式，配置`npm run`,在`package.json`中添加:
```
"scripts": {
  "build": "babel src -d lib"
}
```

## babel-register
babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。

这种方法并不适合正式产品环境使用。 直接部署用此方式编译的代码不是好的做法。 在部署之前预先编译会更好。 不过用在构建脚本或是其他本地运行的脚本中是非常合适的。

babel-register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用。

## 工程中使用
在工程项目中，我们使用的基本是基于 `gulp` 或 `webpack` 的构建的。添加对应的模块，我们就可以在工程中使用 ES6 语法去书写代码了。

### gulp
需要安装: `npm i -D gulp-babel` 模块。

配置 `gulpfile.js` 文件:

```
var gulp = require("gulp");
var babel = require("gulp-babel");

gulp.task("default", function () {
  return gulp.src("src/app.js")
    .pipe(babel())
    .pipe(gulp.dest("dist"));
});
```

### webpack
需要安装: `npm i -D babel-loader babel-core` 模块。

配置 `webpack.config.js` 文件:

```
module.exports = {
    entry: './src/app.js',
    output: {
        path: './bin',
        filename: 'app.bundle.js',
    },
    module: {
        loaders: [{
            test: /\.jsx?$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
        }]
    }
}
```

### 注意点
- 通常项目中，我们还需要添加 `babel-preset-es2015`, 并且设置 `.babelrc` 文件。
- 实际项目运行的时候，为了保证浏览器兼容性，我们也会引入 `babel-polyfill`。
- 添加 `sourcemap` 文件，通常还需要引入其他模块，比如 `gulp-sourcemaps`。