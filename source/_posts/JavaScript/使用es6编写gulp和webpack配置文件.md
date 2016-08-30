title: 使用ES6编写gulp和webpack配置文件
date: 2016-08-30 15:40:10
categories: [JavaScript]
tags: [JavaScript, ES6]
---
ES6 已经可以逐步应用到我们的工程之中了，针对构建工具，我们也可以使用 ES6 来编辑配置文件。比如 `gulpfile` 和 `webpack.config` 文件。
<!--more-->

## gulp
`gulp` 版本在 3.9 以上的，支持我们使用 ES6 语法编写 `gulpfile` 文件。

通过 `gulp -v` 查看项目的 `gulp` 版本，如果低于 3.9, 需要更新。

安装 `babel` 相关库，`npm install babel-core babel-preset-es2015 --save-dev`。

创建 `.babelrc`, 配置：

```
{
    "presets": ["es2015"]
}
```

接下来，我们创建 `gulpfile.babel.js` 替代 `gulpfile.js`，然后我们就可以在此文件中使用 ES6 语法构建响应的 task 了。比如:

```
import gulp from 'gulp';

gulp.task('default', () => console.log('Default task called'));
```


## webpack
类似 `gulp`, `webpack` 也支持使用 ES6 语法去构建配置文件。这是 `webpack` 的一个特性，但是没有列在文档中，在项目的 issue 中，有人回复提及过这个特性。

当我们创建 `webpack.config.[LOADER].js` 时候， webpack 会用相应的 loader 去转换一遍配置文件。使用 `babel` 时，我们需要安装:

```
npm i -D webpack babel-loader babel-core babel-preset-es2015
```

并且创建 `.babelrc` 文件，写入相关配置。

最后，我们创建 `webpack.config.babel.js` 文件，就可以在其中以 ES6 语法去书写了, 比如:

```
import webpack from 'webpack';

export default {
    entry: './src/app.js',
    output: {
        path: './bin',
        filename: 'app.bundle.js',
    }
}
```
