title: Windows下通过Virtualbox安装UbuntuServer1404
date: 2014-05-16 14:00:13
categories: [开发工具]
tags: [Virtualbox, Ubuntu]
---

VPS上的虚拟机安装了Ubuntu Server 14.04。本地主机是Windows的，所以想搭建下本地的开发环境。这里叙述一下安装过程和网络设置。
<!--more-->

## 安装Virtualbox
直接[官网](https://www.virtualbox.org/wiki/Downloads)下载：
1. 客户端
2. Extension Pack

正常安装完毕后，打开VirtualBox->管理->全局设定->扩展，安装Extension Pack。

## 安装Ubuntu Server 14.04
先去[UbuntuServer官网](http://www.ubuntu.com/server)下载iso文件，然后在Virtualbox中新建一个虚拟机，分配好内存，硬盘大小。内存可以是512M或者1G，硬盘大小动态分配。直接启动，然后选择加载iso文件，就he真机一样安装系统了。具体的安装步骤，可以参见[图解UbuntuServer安装过程](http://www.dedecms.com/knowledge/servers/linux-bsd/2012/0819/8387.html)。安装完毕之后就可以直接登陆系统了。

## 网络设置
搭建本地开发环境，最重要的就是要实现宿主机器和虚拟机的互通，联网。

Virtualbox提供了四种网络模式，大体介绍可以参考这篇文章: [VirtualBox虚拟机网络环境解析和搭建-NAT、桥接、Host-Only、Internal、端口映射](http://blog.csdn.net/yxc135/article/details/8458939)。

正常情况下，应该首选桥接模式: 设置->网络->桥接模式。然后再次启动虚拟机，查看虚拟机的IP，应该是和宿主机器在同一网络内，相当于一个真是机器。

有些特殊的情况下，比如无线网卡驱动等问题，导致桥接模式无法使用，具体症状就是虚拟机中无法获取IP。这时候推荐大家选择NAT模式，就是默认的链接方式。这种方式下，虚拟机的IP段和主机不在一起，虚拟机可以访问外网和宿主机器。但是宿主机器无法访问虚拟机。需要设置端口转发，把对宿主主机的访问转发给虚拟机中。

设置端口转发的方式就是：在 设置->网络->网络地址转换(NAT)。高级选项下面，有一个端口转发选项。里面配置上对宿主机器的访问端口， 以及需要转发到虚拟机的端口。比如主机ip填写`192.168.1.60`或者`127.0.0.1`, 主机端口填写`122`, 子系统端口填写`22`。这样ssh访问时候，主机IP填写`192.168.1.60`, 端口选择`122`。就能访问到虚拟机中的`22`端口了。

如果主机ip只填写`192.168.1.60`，就是只能通过ip访问，如要要通过`localhost`和`127.0.0.1`访问，记得再添加一条主机ip为`127.0.0.1`的规则。

## 关于虚拟机分辨率问题
如果不需要图形界面，本人觉得没有必要安装扩展程序(此扩展不同于Extension Pack)。当然如果你要安装的话，我还是说一说吧：

第一次进入系统后，一般会`sudo passwd`设置下root密码。然后替换源。这里给个(更换源的方法)[http://mirrors.163.com/.help/ubuntu.html]。

```
cd /etc/apt
备份软件源文件。
sudo mv sources.list sources.list.backup
输入你的密码
sudo wget http://mirrors.163.com/.help/sources.list.版本名称
sudo mv sources.list.版本名称 sources.list
换好软件源了，更新一下源列表。
sudo apt-get update
接下来从源列表安装更新
sudo apt-get dist-upgrade
```

接下来安装必要开发软件包：
```
sudo apt-get install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)

sudo apt-get install xserver-xorg xserver-xorg-core
```

然后通过光驱加载virtualbox安装目录下的`VBoxGuestAdditions.iso`文件，正常应该会挂在到`/mnt`目录下，如果没有挂在，执行`sudo  mount /dev/cdrom /mnt/`挂载。

进入`/mnt`目录下，执行`sudo ./VBoxLinuxAddion.sh`安装。

安装结束之后，可以点击光驱卸载加载的文件，或者执行`sudo umount /mnt/`

如果是图形界面的话，重启机器分辨率应该就正常了。



---
update:

不知道什么原先，windows的虚拟化服务不能运行了。显示:
```
VT-x/AMD-V 硬件加速在您的系统中不可用。您的 64-位虚拟机将无法检测到 64-位
```

首先检测了BIOS里面是否开启。虽然显示了开启，但是虚拟化软件还是显示未开启。

解决方案：
启动/关闭windows服务->关闭Hyper-V。