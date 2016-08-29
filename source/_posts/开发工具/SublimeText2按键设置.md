title: SublimeText2 按键设置
date: 2013-7-28 16:03:27
categories: [开发工具]
tags: [SublimeText]
---
SublimeText一大优点是操作便捷，自定义性强。它默认的操作方式已经很便捷了，但是我更喜欢添加一些符合自己的操作方式。具体的文件配置我会放在末尾。
<!--more-->

## 光标操作 ##
移动光标是除了打字之外最频繁的操作，我知道sbulime支持VIM操作方式，但是我不是很喜欢VIM的快捷方式，我更喜欢Emacs的操作方式。如果你之前用emacs的话，可以推荐一个插件给大家：[sublemacspro](https://github.com/grundprinzip/sublemacspro)。

其实我对于emacs的操作，最喜欢的是光标移动。所以我自定义了一下emacs的移动方式：

- ctrl+f: 光标向前（forward）
- ctrl+b: 光标向后（back）
- ctrl+p: 光标向上（up）
- ctrl+n: 光标向下（down）
- ctrl+a: 光标到行首
- ctrl+e: 光标到行末
- ctrl+l: 将当前行移动到屏幕中间
- alt+l: 移动到指定行

默认操作：
- ctrl+光标左/右: 按单词移动
- alt+光标左/右: 如果单词是驼峰/下划线链接方式，只能分割移动

## 编辑操作 ##
复制，粘贴，查询也是经常运用到的操作。先说下我自定义的：

- ctrl+t: 新建文件
- alt+a: 全选，因为之前我定义了移动光标和默认的全选冲突了
- alt+f: 查找
- alt+p: 模糊查询
- ctrl+g: 查询并全选
- ctrl+v: 粘贴并缩进
- ctrl+shift+v: 粘贴


默认操作：
- ctrl+左键点击: 添加选择文本
- alt+左键点击: 取消选择文本
- ctrl+shift+t: 打开最近关闭文件
- shift+光标: 选择单个字符
- ctrl+shift+光标: 选择单词

## 代码操作 ##
格式化，缩进等操作。我的自定义：
- ctrl+i: 重新缩进
- alt+/: 自动补全
- ctrl+shift+k: 删除整行
- f5: 打开浏览器

默认操作：
- ctrl+[: 向右缩进
- ctrl+]: 向左缩进
- ctrl+m: 跳转到对应括号
- ctrl+j: 多行转变为一行
- ctrl+shift+l: 光标移动到多行行末
- ctrl+shift+backspace: 删除到行首
- ctrl+回车: 向下添加一行
- ctrl+shift+回车: 向上添加一行


## 插件操作 ##
- alt+e: emmet展开
- alt+j: jsHint
- alt+d: 删除多余空格和空行
- f2: side_bar重命名文件

## Default (Windows).sublime-keymap ##

```
[
{ "keys": ["alt+p"], "command": "show_overlay", "args": {"overlay": "goto", "show_files": true} },
{ "keys": ["alt+f"], "command": "show_panel", "args": {"panel": "find"} },
{ "keys": ["alt+a"], "command": "select_all" },
{ "keys": ["ctrl+t"], "command": "new_file" },
{ "keys": ["f5"], "command": "open_in_browser" },
// add emacs move style
    { "keys": ["ctrl+b"], "command": "move", "args": {"by": "characters", "forward": false} },
    { "keys": ["ctrl+f"], "command": "move", "args": {"by": "characters", "forward": true} },
    { "keys": ["ctrl+p"], "command": "move", "args": {"by": "lines", "forward":
    false} },
    { "keys": ["ctrl+n"], "command": "move", "args": {"by": "lines", "forward":
    true} },
    { "keys": ["ctrl+a"], "command": "move_to", "args": {"to": "bol", "extend": false} },
  { "keys": ["ctrl+e"], "command": "move_to", "args": {"to": "eol", "extend": false} },
  { "keys": ["ctrl+l"], "command": "show_at_center" },
  // Emmet expand
  {"keys": ["alt+e"], "args": {"action": "expand_abbreviation"}, "command": "run_emmet_action", "context": [{"key": "emmet_action_enabled.expand_abbreviation"} ] },
  // select all
  { "keys": ["ctrl+g"], "command": "find_all_under" },
  // jsHint
  {"keys": ["alt+j"], "command": "jshint"},
  // trailing_spaces
  { "keys": ["alt+d"], "command": "delete_trailing_spaces" },
  // enhanced sidebar
  { "keys": ["f2"], "command": "side_bar_rename" },

  // autocomplate
  { "keys": ["alt+/"], "command": "auto_complete" },
  // paste
  // editor配置
    { "keys": ["ctrl+v"], "command": "paste_and_indent" },
    { "keys": ["ctrl+shift+v"], "command": "paste" },
  // reindex
  { "keys": ["ctrl+i"], "command": "reindent" },
  // 光标移动到指定行
  { "keys": ["alt+l"], "command": "show_overlay", "args": {"overlay": "goto", "text": ":"} }
]

```