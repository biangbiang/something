ShadowSocks—科学上网之瑞士军刀
==============================

除去广为人知、人见人爱的VPN，其实还有十八般兵器存在于科学上网界，其中ShadowSocks可以说是其中一把功能齐全的瑞士军刀。服务器端提供了各种版本，如Python、Nodejs、Go、C libev等等，安装配置过程极其简单。而用户端则可以在windows、mac、iOS和android上轻松运行，很好很强大。

PS：此程序开源，感谢作者@clowwindy。

### Shadowsocks是什么？

码农对于shadowsocks应该不陌生，而一般普通网民可能知之甚少。shadowsocks实质上也是一种socks5代理服务，类似于ssh代理。与vpn的全局代理不同，shadowsocks仅针对浏览器代理，不能代理应用软件，比如youtube、twitter客户端软件。如果把vpn比喻为一把屠龙刀，那么shadowsocks就是一把瑞士军刀，轻巧方便，功能却非常强大。

### 喜欢上shadowsocks的理由

很多时候，我们仅仅只是需要上一下google，收个gmail邮件，或者打开某个网站瞄一眼看看有无更新。这种情况下，vpn可以做到吗，可以，但是很麻烦，连个vpn，qq也得掉一次线，有时候还连半天连不上。而通过ss的话呢，后台运行一个小程序，然后浏览器点击切换一下SS的网络，就可以了。不用的时候，再切回来。这也就是其轻巧的地方。

### 如何使用shadowsocks？

* 首先，当然你需要有一个shadowsocks账号，网上有热心网友免费共享出来，比如这个，如果有需要也可以私信我，仅供测试，服务器流量小，禁不起各位大神的折腾。

* windows平台

  1.下载一个shadowsocks的客户端程序（百度网盘下载），不需要安装，解压就可以用。

  2.运行解压后文件夹中的“shadowsocks.exe”，如下图，设置好shadowsocks的账号信息，点save；
  
    ![](http://biang.io/biangpic/blog/bc3d832a88e5366e1c1ab20bc4ebd3b9.png)
  
  3.下载安装chrome的浏览器插件Proxy SwitchySharp（或者百度网盘下载，下载后打开chrome，输入chrome://extensions，把下载的crx文件拖进去即可安装），下载完成后启用之，可以在chrome右上方看到程序的图标，点选项，如下图设置保存；

    ![](http://biang.io/biangpic/blog/60ce21dfc1512abf7982f7089b95e66c.png)
  
  4.此时，可以切换至shadowsocks；

    ![](http://biang.io/biangpic/blog/19f7bce9ede79063e2997c449d4ed2a1.png)
  
  5.接下来，可以在chrome中直接打开youtube试试，测试OK，没问题。

    ![](http://biang.io/biangpic/blog/8317b1dbf8402f17c28469c15a6084f2.png)
  
  如果不需要shadowsocks的网络，选择“使用系统代理设置”即可还原。

* Mac OSX平台

  还是先下载mac下的客户端程序（百度网盘下载，后面的过程和win是一样的，通过chrome的插件使用即可，不再累述。

* iOS平台

  直接在appstore搜索下载shadowsocks（safari直接进入下载），app打开后就是一个浏览器，内置了公共服务器，可以直接输入网址打开youtube了。当然，有时候公共服务器会出现不稳定的情况，这时可以设置自己的服务器使用，设置方法和windows一样。

    ![](http://biang.io/biangpic/blog/be8d291666545861efc7802e60653cbc.png)
  
Android平台
安卓下的软件名称为“影梭”（GooglePlay下载），可以在googleplay中搜索下载，其功能和IOS的基本一样，没有内置浏览器，打开设置好服务器以后，再打开浏览器即可。
