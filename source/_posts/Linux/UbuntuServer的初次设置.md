title: UbuntuServer的初次设置
date: 2014-05-16 15:59:09
categories: [Linux]
tags: [Ubuntu]
---

上一篇说完了Windows下搭建UbuntuServer开发环境，这篇就说说初次进入UbuntuServer的设置。
<!--more-->

## 初次设置
安装UbuntuServer的时候，推荐只选择安装`openssh-server`服务。

进入系统后，首先需要修改root密码，`sudo passwd`。

如果是本地虚拟机跑UbuntuServer，推荐更换软件源：
```
更换软件源
给两个源地址：
http://mirrors.163.com/和http://mirrors.ustc.edu.cn/
给个更换源的方法:(http://mirrors.163.com/.help/ubuntu.html)
cd /etc/apt
备份软件源文件。
sudo mv sources.list sources.list.backup
输入你的密码
sudo wget http://mirrors.163.com/.help/sources.list.lucid
sudo mv sources.list.lucid sources.list
换好软件源了，更新一下源列表。
sudo apt-get update
接下来从源列表安装更新
sudo apt-get dist-upgrade
```

接下来，安装必要的软件和环境:
```
sudo apt-get install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)

sudo apt-get install curl

# 如果之前没有安装openssh
sudo apt-get install openssh-server
```


一般的软件，通过`apt-get`安装即可，版本求新的请通过:
```
./configure && make && make install
```


## node开发环境搭建
最近使用node+express+mongodb制作了一个段地址网站，搭建在UbuntuServer上。所以有必要说下`node`的环境搭建：

### git
`sudo apt-get install git`

### nodejs
通过 PPA 在 Ubuntu 上安装最新版本的 node.js:
```
sudo apt-get install python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

这种安装方式可能存在命名冲突，所以ubuntu下使用的命令是`nodejs`，没特殊必要，可以不修改，如果想要修改为`node`，使用软连接`sudo ln -s /usr/bin/nodejs /usr/bin/node`

不推荐下载github上的代码编译，原因是版本太新，容易和npm上的包不兼容。

### npm
`sudo apt-get install npm`

### mongodb
老老实实按照官方文档来: [点这里](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)。
