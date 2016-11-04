title: SVN 忽略文件的设置
date: 2016-09-20 17:00:00
categories: [开发工具]
tags: [SVN]
---

git 有 `.gitirnore` 文件来设置项目的忽略文件规则，那 SVN 呢？<!--more-->

工作中我有遇到这么一个 SVN 仓库，里面包含了多个项目，不同的人在维护。

而在我的项目文件中，不希望提交某些文件，比如 `node_modules`，之前我的做法是每个版本迁出的时候，手动去添加各个需要忽略的文件。我希望能有一个类似 `.gitignore` 的方式，去维护我 SVN 仓库下需要忽略的文件。

## TortoiseSVN GUI
就是那个 “小乌龟” 软件，一个 GUI 的 SVN 管理工具。

我们常规的做法就是对需要忽略的文件或目录，右击，选择 `TortoiseSVN -> Add to ignore list` （已添加的版本管理中的也类似，菜单名会变为 `Unversion and add to ignore list`）,其中会给出两种忽略方式, 比如 `node_modules` 目录：

- node_modules： 只忽略当前路径下的
- node_modules(recursive)： 忽略整个工程下的

这就是我们常用的设置方式，也是我想要优化的地方。


## TortoiseSVN Command
其实 svn 也是有对应的命令行命令的，比如 `svn status`, `svn update`, `svn add`, `svn commit` 等。对应也有文档： [TortoiseSVN 命令](https://tortoisesvn.net/docs/release/TortoiseSVN_zh_CN/tsvn-cli-main.html)

其中就有介绍如何加入忽略列表:

```
svn propget svn:ignore PATH > tempfile
{edit new ignore item into tempfile}
svn propset svn:ignore -F tempfile PATH
```

这就是命令行下添加忽略规则的方式，我们可以看到实际是加到项目的 `properties` 中，这里使用的一个文件去维护忽略规则，**这其实就是我想要的方式**。

导入忽略规则后，我们在项目中右击，通过 `TortoiseSVN -> Properties` 可以查看，修改，以及导入导出。关于忽略的两种方式，实际对应的属性也是两个：

- `svn:ignore`
- `svn:global-ignore`


## 优化方案
通过命令行的方式，我们可以看到忽略规则是可以被导入导出的。这样我们就可以通过文件去维护项目的忽略列表，虽然这个忽略规则不会自动被识别，但是已经可以简化我们许多人工点击的操作了。

操作如下：
- 生成忽略文件, 两种方式：
    - 通过 `TortoiseSVN GUI` 方式，点击操作设置相关忽略文件
    - 维护 `.svnignore` 和 `.svnignore-global` 两个文件，分别通过命令行导入
- 在项目中右击，通过 `TortoiseSVN -> Properties` 对配置导出
- 在新项目中， 通过 `TortoiseSVN -> Properties` 导入配置

配置导出的时候，注意全选，然后 `export` ，导出的文件是 `.svnprops`。

我们也可以只导出忽略规则，方式到对应的 `.svnignore` 和 `.svnignore-global` 文件中。

## 参考
- [Tortoisesvn: 忽略文件和目录](https://tortoisesvn.net/docs/release/TortoiseSVN_zh_CN/tsvn-dug-ignore.html)
- [Tortoisesvn 命令: 加入忽略列表](https://tortoisesvn.net/docs/release/TortoiseSVN_zh_CN/tsvn-cli-main.html#tsvn-cli-addignore)
- [How do I ignore files in Subversion?](http://stackoverflow.com/questions/86049/how-do-i-ignore-files-in-subversion)
