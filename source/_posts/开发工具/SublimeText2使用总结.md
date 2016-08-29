title: SublimeText2 使用总结
date: 2013-7-20 22:22:22
categories: [开发工具]
tags: [SublimeText]
---
SublimeText是我现在主要使用的编辑器,它有很多优点，如果你有兴趣，可以慢慢看完。
<!--more-->

# 初次使用 #
安装好了SublimeText之后，我一般会调节一下基本设置。包括主题，配色，字体等。在安装主题之前，一般还是要安装下package control来进行插件管理。安装地址[Sublime Package Control](http://wbond.net/sublime_packages/package_control/installation)。安装完成之后，通过`ctrl+shift+p`开打控制面板，使用`install package`安装主题和配色。主题是SublimeText的外观，配色是代码的高亮方案。我推荐的主题是Theme - Phoenix, 配色方案是Dayle Rees Color Schemes, 字体我使用的yaheiconsolashybrid。贴个图片大家欣赏下吧:
![sublime](http://ww3.sinaimg.cn/large/6810c776jw1e6tqzlz33nj20sg0gr0vi.jpg)

# 配置设置 #
首先说明配置文件的路径，点击Preferences->Browse Packages打开目录，找到`User`目录，这里的文件就是自己的配置文件，最好备份，方便下次替换。

具体的配置，一般是Preferences->Settings-Default和Key Bindings-Default。这两个最好不要修改，如果要替换，把相应的参数写入到Settings-User和Key Bindings-User里，会自动覆盖Default相同属性。

# 特色功能 #
1. ctrl+p，搜索。这个搜索可以左侧的Folders里所以文件，而且是模糊搜索，不需要完整的文件名。配合`#`, `@`, `:`可以搜索变量，函数，行数。
2. 多行编辑。按住ctrl加左击，可以出现多个光标位置。
3. 多重选择， `ctrl+d`可以多重选择，结合光标键，可以批量修改。
4. 多屏编辑，alt+shift+数字键。
5. Projects，通过View->Side Bar->show Side Bar左侧文件结构管理。
6. snippet, 不同格式的文件，可以设置不同的snippet,就是简写，通过tab扩展成相应的内容。
7. 各种插件支持
8. 正则表达式搜索,比如我要删除所有的空行，可以使用`^[\s]*\n`来选择所有空行。可以使用`(?<=<h2>).+(?=</h2>)`来匹配h2标签内的内容。
9. ctrl+shift+p，功能菜单。只有你想不到，没有做不到的事情。

# 技巧 #
## 操作技巧 ##
1. 选择多行，按ctrl+shift+L使得光标在每一行最后。
2. 被包裹的文本，可以直接添加括号，引号等。
3. ctrl+shift+光标左右移动，选择文本到行首、行末。
4. 对于左边的side bar，可以新建Projects, 管理文件夹。参数path为路径，file_exclude_patterns为不需要添加的文件, folder_exclude_patterns为不需要添加的目录。
5. 善用功能搜索，比如`copy`。

## 创建snippet ##
通过Tools->new snippet创建新的snippet, 保存的时候，文件名后缀为.sublime-snippet，注意内容里面`$0`是光标最后出现的位置。我一般会保存到User目录下，方便同步。给出一个jquery的实例：


    <snippet>
        <content><![CDATA[
    \$('#${1:function_name}')${0};
    ]]></content>
        <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
        <tabTrigger>jqi</tabTrigger>
        <!-- Optional: Set a scope to limit where the snippet will trigger -->
        <scope>source.js</scope>
        <description>$('#')</description>
    </snippet>

作用是通过`jqi`扩展成为`$('#');`的样子。


## 创建Build命令 ##
通过Tools->build->new ，创建一个build命令，保存在User下，可以建立一个build文件夹管理。
