title: Vim 和 spf13-vim 配置安装
date: 2016-11-27 16:03:27
categories: [开发工具]
tags: [Vim]
---


前几周折腾 spacemacs 后，发现内置的 vim 模式很值得学习，所以干脆先折腾折腾 Vim 吧。也不想从零开始了，直接 github 搜了下，发现 [spf13-vim](https://github.com/spf13/spf13-vim) 是一个比较不错的配置。
<!--more-->


# Vim
[Vim](http://www.vim.org), Download 页面中有各个系统的安装说明，vim 和 emacs 一样，都是提供源码，自己去编译。所以一般都会有第三方提供编译过的各个系统版本，适合安装或直接使用。

## windows
- [Vim github 源码](https://github.com/vim/vim/releases)
- [Yongwei's Programming Page](http://wyw.dcweb.cn/index.htm#download): 他人编译的版本
- [Vim builds for Windows](https://tuxproject.de/projects/vim/): 一个日本人编译的版本
- [vim/vim-win32-installer](https://github.com/vim/vim-win32-installer/releases): 对官方进行编译的一个版本，我个人在使用，推荐


## Mac
- [MacVim](https://github.com/macvim-dev/macvim): 建议通过 brew 安装
- [如何替换终端中的vim版本](http://stackoverflow.com/questions/21961872/update-osx-vim-using-macvim): brew 安装时候增加 `--override-system-vim` 选项。

## 如何选择带 lua 版本
进入 vim 后，使用 `:lua print('x')` 测试是否支持 lua。如果编译的版本支持，会提示你是否缺少依赖，比如在 windows 下，会提示缺少 `lua53.dll`, 然后你需要这个根据你的系统，去 (Lua Binaries 找对应lua版本, 再进入（比如 64位 windows ） Browse /5.3.3/Windows Libraries/Dynamic at SourceForge.net 找到 lua-5.3.3_Win64_dll10_lib.zip 或 lua-5.3.3_Win64_dllw4_lib.zip 下载 （区别在页面下面有解释）。

解压缩后，将对应的 lua53.dll 放入在 gvim.exe 同级目录中，再次运行 gvim.exe, 查看 `:echo has("lua")` 这时应该返回就为1了。


# spf13-vim
这是一个配置文件，整合了许多插件和配置，比较适合大众使用，如果你之前没接触过 vim ，不如用它来上手。

## 安装
### windows
任何开源工具在 windows 下的安装，都是比较麻烦的。。。spf13-vim 需要依赖：
- vim-with-lua: 带 lua 的vim 版本，因为内置了 neocomplete 插件
- git: 需要git clone 配置文件仓库
- curl: 命令，需要下载安装文件

spf13-vim 提供了一个批处理，[spf13-vim-windows-install.cmd](https://github.com/spf13/spf13-vim/blob/3.0/spf13-vim-windows-install.cmd), 可以下载下来通过管理员权限运行。

安装过程会下载插件，使用的是 vundle 管理插件的。你可以中断安装，进入 vim 后，`:PluginInstall` 进行安装。

### Mac
`curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh` 即可。

# 配置管理
vim本身的配置会被 spf13-vim 通过软链接管理，我们自定义的配置，推荐维护：

- .vimrc.local
- .gvimrc.local
- .vimrc.before.local
- .vimrc-bundles.local

可以将这些文件，放置到你的 dotfiles 中管理。
