title: nginx安装与反向代理
date: 2014-05-19 17:50:55
categories: [服务器]
tags: [nginx]
---

想要在服务器上搭配N多站点，使用反向代理，监听80端口是个好方法，并且Nginx的效率很高。

<!--more-->


## Window
[下载地址](http://nginx.org/en/download.html).选择`nginx/Windows-1.7.0`下载。

解压缩到本地，这里我选择`D:\Servers`下，解压之后完整路径为`D:\Servers\nginx-1.7.0`。

在`D:\Servers\nginx-1.7.0`下,启动使用`start nginx`。

是否启动可以通过`tasklist | grep nginx`查看。记住，不要多次启动，不然后台会挂很多nginx的。

## Linux
`sudo apt-get install nginx`安装即可。安装完毕默认启动。


## 设置反向代理
这里举个例子，就是通过本机ip访问的转到3000端口，localhost访问的转到4000端口。下面看具体设置。

### Windows
配置文件目录在`D:\Servers\nginx-1.7.0\conf`下， 修改`nginx.conf`文件，去除其中的server配置，添加上`include    vhosts/*.conf;`。

在`D:\Servers\nginx-1.7.0\conf`下创建`vhosts`目录，里面方式各个server配置即可。

这里给一个我的配置（使用localhost访问4000端口， 192.168.1.60访问3000端口）的例子：
```
upstream nodejs__upstream {
     server 127.0.0.1:3000;
     keepalive 64;
}
server {
     listen 80;
     server_name 192.168.1.60;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream;
     }
}


upstream nodejs__upstream2 {
     server 127.0.0.1:4000;
     keepalive 64;
}
server {
     listen 80;
     server_name localhost;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream2;
     }
}
```

### Linux
配置文件目录在`/etc/nginx`中。

Linux下的配置文件结构和Windows略有不同，但是意义相似。修改`/etc/nginx/nginx.conf`文件：
```
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;

# 修改为
include /etc/nginx/conf.d/*.conf;
# include /etc/nginx/sites-enabled/*;
include /etc/nginx/vhosts/*.conf;
```
注意这里，我新建了个目录`vhosts`，如果不想新建，就把里面的配置文件放到`conf.d`或者`sites-enabled`中。

`vhosts`目录下我包含了两个文件，`default.conf`和`proxy-node.conf`。

#### default.conf
此文件就是`sites-enabled`下的`defalut`文件。

#### proxy-node.conf
```
upstream nodejs__upstream {
     server 127.0.0.1:3000;
     keepalive 64;
}
server {
     listen 80;
     server_name 192.168.1.60;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream;
     }
}


upstream nodejs__upstream2 {
     server 127.0.0.1:4000;
     keepalive 64;
}
server {
     listen 80;
     server_name localhost;
     location / {
         proxy_set_header   X-Real-IP            $remote_addr;
         proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
         proxy_set_header   Host                   $http_host;
         proxy_set_header   X-NginX-Proxy    true;
         proxy_set_header   Connection "";
         proxy_http_version 1.1;
         proxy_pass         http://nodejs__upstream2;
     }
}
```

保存完毕之后，重启nginx。 `sudo nginx -s reload`。

之后通过`localhost`和`192.168.1.60`访问就能对应分别的端口了。

----
这里说下node做服务器时listen的设置，只绑定端口，无需绑定ip。如果像官网上写的:
```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

那么只能通过`localhost:1337`和`127.0.0.1:1337`访问。无法通过`192.168.1.60:1337`这样访问。