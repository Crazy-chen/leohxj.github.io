title: spacemacs 初体验
date: 2016-11-04 16:00:00
categories: Emacs相关
tags: [Emacs]
---



前段时间听了代码时间的一期播客: [emacs 陈斌](https://codetimecn.com/episodes/emacs) 最近又看到了 [spacemacs](http://spacemacs.org/)： 一个结合 Emacs 与 Vim 方式的全局配置，就像 [oh-my-zsh](http://ohmyz.sh/) 基于 zsh，简单的安装既让你有种老司机的感觉。<!--more-->

## 安装 Emacs
spacemacs 是基于 Emacs 的配置。所以我们要先安装 Emacs, 关于 Emacs 版本又有很多，官方下载的算是基础版本，也有一些第三方编译的版本，包含了一些常用的模块，比如图片模块。 spacemacs [推荐安装的版本](https://github.com/syl20bnr/spacemacs#prerequisites) 也是基于编译的，而不是 GNU Emacs 官方提供的下载。

我在 Windows 和 MacOS 下都安装了 Emacs:
- Windows 下使用的就是: [emacs-w64-25.1-O2-with-modules.7z](https://sourceforge.net/projects/emacsbinw64/files/release/)
- MacOs 下是通过 brew 安装的

PS: 而在后续的使用过程中，其实还需要安装好 git, cygwin 等工具。

## 安装 spacemacs
由于 spacemacs 就是 emacs 的配置，所以安装它其实就是替换 `.emacs.d` 和 `.emacs`。

1. 先备份, 并删除 `.emacs` 文件：

```
cd ~
mv .emacs.d .emacs.d.bak
mv .emacs .emacs.bak
```

2. clone 项目到 `.emacs.d` 目录：

```
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

3. 启动 Emacs, 这里会进行 spacemacs 的初始化，问答形式完成一些基础配置， 直接全部回车即可，生成 `.spacemacs` 配置文件。

4. 如果网络没有问题，会直接更新所有需要的模块

5. (可选) 下载并安装 [Source Code Pro 字体](https://github.com/adobe-fonts/source-code-pro)

当然，多数国内环境下，网络会出问题。所以我们可以在完成第三步后，退出 Emacs， 去修改配置文件 `.spacemacs`:

- `dotspacemacs-elpa-https`: 设置为 `nil`, 让其走 http 协议
- 可添加国内镜像地址: 在 dotspacemacs/user-init 中添加:
    ```
    (setq configuration-layer--elpa-archives
        '(("melpa-cn" . "http://elpa.zilongshanren.com/melpa/")
          ("org-cn"   . "http://elpa.zilongshanren.com/org/")
          ("gnu-cn"   . "http://elpa.zilongshanren.com/gnu/")))
    ```
- 清空 `.emacs.d/elpa` 目录
- 重启 Emacs

安装模块后，下次进入启动的时候就会比较短， 欢迎界面每次会展示所有模块加载时间, 比如: `213 packages loaded in 4.740s (e:155 r:2 l:10 b:46)`, 这后面的几个字符，我单独解释下:

- e: elpa stats
- r: recipe stats
- l: local stats
- b: build-in stats


## 基本操作
在 spacemacs 下有个很重要的概念，叫做引导键，用来出发组合性质的功能， `SPC` 就是这个引导键，对应键盘的是空格键。

光标移动使用的是 `vim` 模式， 如有不熟悉的，可以在主界面点击 ? -> `evil tutor` 学习。

一些常用的，我先列在这里：

- 返回主页面: `SPC-b-h`
- 新建缓冲区: `SPC-b-N`
- 保存缓冲区: `SPC-f-s`
- 显示所有缓冲区: `C-x C-b`
- 切换窗口: `C-x o`
- 退出: `C-x C-c`


## 参考
- [Emacs sexy](http://emacs.sexy/)
- [emacs 陈斌](https://codetimecn.com/episodes/emacs)
- [GNU Emacs](https://www.gnu.org/software/emacs/)
- [spacemacs](http://spacemacs.org/)
- [Emacs China: ELPA 镜像](http://elpa.emacs-china.org/)
