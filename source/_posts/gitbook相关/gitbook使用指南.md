title: gitbook 使用指南
date: 2016-9-17 16:00:07
categories: gitbook相关
tags: [gitbook]
---

我之前使用 `gitbook` 的时候还是 `1.x.x` 版本，现在都已经 `3.x.x` 了。<!--more-->

## 什么是 GitBook
一个致力于解决文档，书籍书写与发布的技术方案, 于 2014 年中旬创立，建立了一套开源书籍的书写规范和相关的构建工具。后来也创立了一个网站， [GitBook.com](https://gitbook.com/), 供世界各地的人发布和管理自己的书籍。

## 安装 GitBook
`GitBook 3.x.x` 版本需要 `node >=4.0.0`, 这里插一句，大多数情况下，node 还是推荐使用 LTS 版本，最新版本的特性有些库支持还是有问题的，而且会存在不兼容的情况。

通过 `npm install gitbook-cli -g` 全局安装 `gitbook` 命令，安装完成后，有几个常用的命令:

- `gitbook --version`: 查看当前使用的版本
- `gitbook ls`: 系统存在的 gitbook 版本
- `gitbook ls-remote`: 所有 gitbook 版本
- `gitbook fetch`: 下载对应的 gitbook 版本
- `gitbook current`: 当前目录使用的 gitbook 版本

当前版本下，默认安装的是:

```
CLI version: 2.3.0
GitBook version: 3.2.0
```

如果我们需要使用不同的版本，需要自己安装，或者通过 `book.json` 配置 `gitbook` 版本。比如：

```
{
    "gitbook": "~2.x.x"
}
```

这样在含有 `book.json` 的目录下运行 `gitbook` 相关命令，就是基于指定的版本了。

### 查看相关命令
通过 `gitbook` 和 `gitbook help` 可以查看相关的命令。

## 创建书籍
`gitbook init <folder>` 创建项目， 生成:

- README.md
- SUMMARY.md

如果对项目需要配置，还需自行添加 `book.json` 文件。

## 运行书籍
`gitbook serve` 命令。

## 编译书籍
`gitbook build` 命令。

## SUMMARY.md
`SUMMARY.md` 是用来定义书籍的章节的，用来生成目录。比如:

```
# Summary

* [Introduction](README.md)
* [Part I](part1/README.md)
    * [Writing is nice](part1/writing.md)
    * [GitBook is nice](part1/gitbook.md)
* [Part II](part2/README.md)
    * [We love feedback](part2/feedback_please.md)
    * [Better tools for authors](part2/better_tools.md)
```

在 `2.x.x` 版本中，这样会生成目录:

```
Introduction
1.Part I
    1.1 Writing is nice
    1.2 GitBook is nice
2.Part II
    2.1 We love feedback
    2.2 Better tools for authors
```




`3.x.x` 版本后，默认推荐不为章节编码，生成的内容如下：

```
Introduction
Part I
    Writing is nice
    GitBook is nice
Part II
    We love feedback
    Better tools for authors
```

如果你想要展示章节编码，需要添加设置, 修改 `book.json`:

```
"pluginsConfig": {
       "theme-default": {
           "showLevel": true
       }
   }
```

这样生成的目录会是这样:

```
1.1 Introduction
1.2 Part I
    1.2.1 Writing is nice
    1.2.2 GitBook is nice
1.3 Part II
    1.3.1 We love feedback
    1.3.2 Better tools for authors
```

所有内容都会以 `1.x` 编码开始。原因是我们没有使用 `Part` 分割章节， 这也是 `3.x.x` 版本后推荐的做法，我们修改 `SUMMARY.md`:

```
# Summary

* [Introduction](README.md)

# Part I
* [Part I](part1/README.md)
    * [Writing is nice](part1/writing.md)
    * [GitBook is nice](part1/gitbook.md)

# Part II
* [Part II](part2/README.md)
    * [We love feedback](part2/feedback_please.md)
    * [Better tools for authors](part2/better_tools.md)
```

这样之后，显示的编码会变成这样:

```
1.1 Introduction
2.1 Part I
    2.1.1 Writing is nice
    2.1.2 GitBook is nice
3.1 Part II
    3.1.1 We love feedback
    3.1.2 Better tools for authors
```

不带编码显示是这样, 会带上分割线:

```
Introduction
-----
1.Part I
    1.1 Writing is nice
    1.2 GitBook is nice
-----
2.Part II
    2.1 We love feedback
    2.2 Better tools for authors
```

## PAGES
每篇文章，就是一个单独的 `markdown` 文件，支持 [GitHub Flavored Markdown syntax](https://guides.github.com/features/mastering-markdown/), 比如:

```
# Title of the chapter

This is a great introduction.

## Section 1

Markdown will dictates _most_ of your **book's structure**

## Section 2

...
```

## 发布
你可以选择在 [GitBook New](https://www.gitbook.com/new) 上创建，选择在线编辑或者通过 `git clone` 下载到本地编辑。

你还可以将在 Github 上创建的书籍，同步到 GitBook 中， 在 GitBook 的项目设置中，手动添加对应的 Github 仓库，注意要保证 GitBook 对你的 Github 有访问权限。

原理是借助 Webhook, 当你的 github 仓库有提交时，同步 GitBook 仓库。

## 绑定域名
默认发布在 GitBook 上的项目，URL规则是这样的: `https://www.gitbook.com/book/{username}/{bookname}`, 如果我们要为此绑定域名，需要两处设置:

- GitBook对于的项目设置中，修改 `Domain`, 添加自定义域名
- 在你的域名服务商提供的控制面板中，设置 `CNAME`， 绑定 `www.gitbooks.io`，开启 `domain forwarding`



## 缘由
重新使用 GitBook 是因为上周有人发邮件给我，说放在 GitBook 上的 [<程序员的自我修养>](https://www.gitbook.com/book/leohxj/a-programmer-prepares/details) 这本书，下载为 PDF格式时字体有些怪异，希望我能将书开源到 github 上供他自己编译，考虑到此书确实也很久没有更新了，我决定借此机会重启这一系列的更新，希望能将自己这一两年积累的知识贡献出来。

## 参考
- [GitBook 官网](https://www.gitbook.com/)
- [GitbookIO/gitbook](https://github.com/GitbookIO/gitbook)
- [GitBook Document](http://toolchain.gitbook.com/)
- [Gitbook Help Center](https://help.gitbook.com/)