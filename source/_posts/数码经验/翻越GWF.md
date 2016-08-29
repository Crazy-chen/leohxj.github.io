title: 翻越GWF
date: 2014-04-15 14:16:17
categories: [数码经验]
tags: [Windows]
---
在国内，特别是程序员，是需要查阅很多国外资料的。但是国内的限制你懂的。今天就来说说如何绕过这些限制。
<!--more-->

## 免费的方案
`goagent`, [goagent项目地址][1]。这是利用Google Appengine设置代理，好处就是免费，达到浏览器翻墙的效果。很久以前，我一直是用它，很稳定，适合学生。

具体使用方法，项目里面介绍的很详细，按步骤来就可以。

## SSH
`Secure Shell`的缩写，是一种建立在应用层和传输层基础上的安全协议。主要用来远程连接，也可以作为代理使用，推荐配合clowwindy的`shadowsocks`使用。[shadowsocks项目地址][2]。

shadowsocks连接方式配合chrome插件`proxy switchSharp`，能很好的实现分流（翻墙的才走代理）。

我有一点疑问关于SSH的连接，貌似不用输入用户名。只提供ip，端口和密码即可？

## VPN
这个是最普遍的翻墙方式，但是所有代理都会走VPN。对于有流量限制和下载的情况来说，需要来回切换，很麻烦。所以我找到了一下如果让VPN实现分流的操作，这里提供一些方式：

### PAC
pac是系统级别的代理配置，属于应用层，所以所有应用都会走代理，通过PAC进行判断，可以实现分流操作。开启之后，所有请求会先走VPN再PAC判断，走两层代理。

这里有一个更优的`GFWList`，也是由clowwindy大神提供的，[gwflist2pac][3]。


### 修改路由表
如果让VPN做动态判断，只能在开启VPN后，修改系统的路由表。可以查看chnroutes, [chnroutes项目地址][4]。MacTalk君有一篇[文章][6]可以参考.

chnroutes会定期更新，这里提供一个下载地址[chnroutes定期更新下载][5]。


## 一些资源
- [Cow](https://github.com/cyfdecyf/cow)
- [东哥的服务](https://vpnso.com/)
- [目前自用的VPN](https://www.runos.us/)

[1]: https://code.google.com/p/goagent/
[2]: https://github.com/clowwindy/shadowsocks/wiki/Ports-and-Clients
[3]: https://github.com/clowwindy/gfwlist2pac
[4]: https://github.com/fivesheep/chnroutes/wiki
[5]: http://chnroutes-dl.appspot.com/
[6]: http://macshuo.com/?tag=chnroutes