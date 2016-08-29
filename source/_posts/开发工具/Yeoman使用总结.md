title: Yeoman使用总结
date: 2013-7-21 18:04:35
categories: [开发工具]
tags: [开发工具, Yeoman]
---

Yeoman发布了1.0。正好最近也使用到了，就总结一下。<!--more-->

# 初次使用 #
初次使用的话，根据官方的向导，首先进行安装。原来的版本需要`npm install -g grunt-cli bower yo`完安装，现在只需`npm install -g yo`即可自动安装`grunt`和`bower`了。

接下来，安装模板文件，比如`npm install -g generator-webapp`，这样在命令行下我们就可以通过`yo webapp`来使用了。

在一个目录下，通过`yo webapp`会导入模板，正常情况下回自动执行`npm install && bower install`如果没有执行，那么请手动执行。这里需要注意的是`bower install`，因为PC下bower默认通过http协议，需要修改为git协议才能正确安装文件，在命令行下通过`git config --global url."https://".insteadOf git://`设置。如果一切正常的话，可以执行`grunt`命令，它会完成整个工程的编译，测试，打包等功能。

# 自定义模板 #
虽然Yeoman提供了不少模板，但是我还是希望有一个自己定义的模板，Yeoman支持自定义功能，需要首先安装`npm install -g generator-generator`。

## 建立自己的模板 ##
建立一个以`generator-`开头的目录，比如`generator-blog`。在目录下执行`yo generator`。现在就完成了模板的建立，接下来需要我们自己来完成自定义了。这里的文件结构我说明一下：

- app: 是模板内容，就是yo拷贝过去的东西（核心)
- package.json: 项目的信息，如果你想要发布到npm上，请按照规格去写
- README.md: 项目的介绍信息，自行填写

接下来，执行`npm link`就在本地创建了一个模板，可以通过`yo blog`调用了。

## 定义模板内容 ##
模板文件的内容都在app目录下，`index.js`是核心，完成命令行的交互，文件的拷贝等功能。对于这个文件，如果我们想要增加交互的内容，那么在`var prompts = []`中添加对象，比如`[{name: 'appName', message: 'What do you want to name your app?', default: 'yourAppName'}]`。并且在

```
this.prompt(prompts, function (props) {
    // this.someOption = props.someOption;
    this.appName = props.appName;
    cb();
  }.bind(this));
```
中完成注册。这样我们就可以通过`this.appName`或者`<%= appName %>`来访问了。

接下来，对于templates目录，我们可以在里面通过`<%= xxx %>`的形式，设置需要替换的内容。然后在:

```
BlogGenerator.prototype.app = function app() {
  this.template('_README.md', 'README.md');

  this.template('_package.json', 'package.json');
  this.template('_bower.json', 'bower.json');
  this.template('_config.json', 'config.json');

  this.template('_Gruntfile.js', 'Gruntfile.js');

  this.template('bowerrc', '.bowerrc');
  this.template('editorconfig', '.editorconfig');
  this.template('gitignore', '.gitignore');
  this.template('jshintrc', '.jshintrc');

  this.directory('test/', 'test/');
  this.directory('src/', 'src/');
};
```
中通过template拷贝，如果`gruntfile.js`里面已经有了`<%= xxx %>`的形式，那么通过`<%%= xxx =>`再次转义即可。

## 发布到npm ##
首先需要注册一个npm账号，然后再命令行下通过 `npm address` 注册，在项目的目录下执行 `npm publish`，如果`package.json`文件正确，会自动发布到npm上。你只需要把代码同步到github上即可，但是npm并不是从github上拖取，所以每次更新，都需要更改版本号，然后再次`npm publish`。

# 参考资料 #
- [Yeoman官网](http://yeoman.io/)
- [前端项目可以更简单—Yeoman入门指南](http://www.36ria.com/6144)
