title: 搭建Node+Express+Mongodb环境
date: 2014-05-19 14:16:05
categories: [Nodejs]
tags: [nodejs, express, mongodb]
---

每次搭建都要查一遍，不如记录一下。
<!--more-->

## Windows
先说下Windows下必要的开发环境，最好安装上`Microsoft Visual Studio`， 保证系统有`python 2.7.x`。因为很多npm的包，需要这些编译环境。

### Nodejs
window下node直接通过官网下载, [下载地址](http://nodejs.org/download/)。下载msi文件直接安装即可，它会自动添加`node`和`npm`到系统环境变量中。

以后如果要切换版本，直接下载对应版本，重新安装即可。

### Express
截至2014年5月19日， Express的版本维持在4.X。不同于之前的express，4.x分割出去很多模块，比如`connect`和`generator`。

安装方法:
```
npm install -g express

# 如果需要安装之前的版本, 加个@符号和版本即可
npm install -g express@3

# 4.x版本无法直接通过express命名新建项目，所以需要安装
npm install -g express-generator
```

### Mongodb
先给一个官方的文档：[点我](http://docs.mongodb.org/manual/)。里面有各个系统的安装方式。

下面说说我的安装方式：

第一，在`D:`根目录下创建一个`DB/MongoDB`文件夹， 把对应版本的mongodb文件加载到此目录下的一个目录中。比如我这里的`MongoDB-2.6.0`。然后分别建立两个目录存放数据库和日志:`MongoDB-2.6.0-db`和`MongoDB-2.6.0-log`。并且保证`MongoDB--2.6.0-log`下有一个`mongodb.log`文件。

第二，使用管理员权限打开cmd， `win+X -> A`。然后:
```
echo logpath=D:\DB\MongoDB\MongoDB-2.6.0-log\mongo.log> "D:\DB\MongoDB\MongoDB-2.6.0\mongod.cfg"
echo dbpath=D:\DB\MongoDB\MongoDB-2.6.0-db>> "D:\DB\MongoDB\MongoDB-2.6.0\mongod.cfg"
```

这一步是写安装配置文件，正确处理后，会在`D:\DB\MongoDB\MongoDB-2.6.0`目录下多个`mongod.cfg`文件，里面内容如下:
```
logpath=D:\DB\MongoDB\MongoDB-2.6.0-log\mongo.log
dbpath=D:\DB\MongoDB\MongoDB-2.6.0-db
```

最后，安装使用命令：
```
sc.exe create MongoDB binPath= "\"E:\DB\MongoDB\MongoDB-2.6.0\bin\mongod.exe\" --service --config=\"E:\DB\MongoDB\MongoDB-2.6.0\mongod.cfg\"" DisplayName= "MongoDB 2.6 Standard" start= "auto"
```

安装结束会提示`[SC] CreateService SUCCESS`, 然后可以通过`net start MongoDB`启动。默认是会自动启动的。如果出问题，可以重启先看看，不行再重新安装。

#### 默认的使用方法
如果不添加系统服务，启动`mongodb`的步骤是这样的：

打开cmd命令行，进入D:/DB/MongoDB/MongoDB-2.6.0/bin目录，输入如下的命令启动mongodb服务， 需要指定数据库位置：
```
D:/DB/MongoDB/MongoDB-2.6.0/bin>mongod.exe --dbpath D:/DB/MongoDB/MongoDB-2.6.0-db
```

保持这个cmd窗口不关闭，再打开一个cmd窗口， 输入：
```
D:/DB/MongoDB/MongoDB-2.6.0/bin>mongo
```

这样才能运行mongo服务。


## Linux
Linux下首先需要必要的环境:
```
sudo apt-get install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)

sudo apt-get install python-software-properties python g++ make

sudo apt-get install git
```

### Nodejs
通过 PPA 在 Ubuntu 上安装最新版本的 node.js：
```
sudo apt-get install python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```
可能会存在命名冲突，是的安装之后使用`nodejs`而非`node`，如果需要修改，创建链接：
```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

其实使用`nodejs`也没关系，我修改主要是想windows和linux下命名统一。

### NPM
Linux下npm好像不是随着`node`安装的，需要:
```
sudo apt-get install npm
```

### Express
安装了`npm`之后，安装方式和windows下一致：
```
npm install -g express

# 如果需要安装之前的版本, 加个@符号和版本即可
npm install -g express@3

# 4.x版本无法直接通过express命名新建项目，所以需要安装
npm install -g express-generator
```

### Mongodb
这个还是推荐看官方文档：[点我](http://docs.mongodb.org/manual/)。

我的安装方式是：
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list

sudo apt-get update


sudo apt-get install mongodb-org
```
