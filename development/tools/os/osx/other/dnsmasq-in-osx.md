在Mac OS X上安装dnsmasq来支持hosts泛解析
========================================

最近访问谷歌系列网站越来越慢了，不探究原因，但这让我感到非常苦恼，如果连Google都使用不了的话，那我上网的意义就失去了一半了。不过好在我们有hosts，可以实现简单的翻# 墙，来解决大多数谷歌服务的访问问题。可是谷歌的域名实在是太多了，我们有没有什么办法来实现类似*.google.com/*的泛解析呢？很可惜，传统的hosts文件不可以行。但是有一个叫做dnsmasq的软件，它能提供类似DNS缓存的功能，可以用它来实现很强大的泛解析功能。

下面以OS X Mavericks做例子，来讲述如何安装使用DNSMASQ，来实现谷歌服务直连。

### Mac安装DNSMASQ

要安装dnsmasq，你需要先安装Homebrew。

它自称“OS X 不可或缺的套件管理器”。

### 安装Homebrew

请打开终端（应用程序>实用工具），并运行

    ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

你需要按照提示来继续安装。这个过程或许会很慢，受限于网络状况。如果实在很慢，你可以在VPN环境下安装。

安装完成之后，你就可以使用brew命令来安装dnsmasq了。

### 安装并配置dnsmasq组件

仍然在终端运行

    brew install dnsmasq

等待安装完成后，请在/usr/local/文件下新建一个etc文件夹。

现在把/usr/local/opt/dnsmasq/dnsmasq.conf.example文件拷贝至并重命名为/usr/local/etc/dnsmasq.conf。

在刚刚的/usr/local/etc/文件夹下新建一个resolv.dnsmasq.conf文件。

用sublime text，textmate，bbedit等纯文本编辑器打开这个resolv.dnsmasq.conf文件，输入以下内容

    nameserver 8.8.8.8

    nameserver 8.8.4.4

    nameserver 42.120.21.30

    nameserver 168.95.1.1

这些都是你常用的DNS地址，你可以添加更多，比如OpenDNS。

用sublime text，textmate，bbedit等纯文本编辑器打开同文件夹下的dnsmasq.conf文件，增加以下内容

    resolv-file=/usr/local/etc/resolv.dnsmasq.conf

    strict-order

    no-hosts

    cache-size=32768

    listen-address=127.0.0.1

这就是最简单的配置文件。需要说明的是，listen-address后面的可以是多个IP用英文逗号隔开，例如你写listen-address=127.0.0.1,192.168.1.102，其中192.168.1.102是你的计算机的内网IP，就可以实现同一个局域网内的设备，通过设置DNS为这个IP，来实现都通过你的dnsmasq来查询dns，即局域网范围内的DNS泛解析。

要运行并且开机自动运行dnsmasq，在终端运行

    sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons

    sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist

现在，你可以通过把Mac系统的DNS改为127.0.0.1来使用dnsmasq。同局域网的用户也可以修改DNS到此台Mac的IP即可。前提是要把此台Mac的局域网IP写到listen-address里。

![](http://biangbiangpic.b0.upaiyun.com/blog/9c7e36164e655d7d5c9b3f69074dab0a.png)

要检查运行情况，你可以在终端运行

    dig g.cn

来检查是否在使用本地的dnsmasq进行dns解析。

### DNSMASQ 泛解析

上面只是安装好了dnsmasq，下面来具体介绍DNSMASQ的泛解析功能，来突破墙，实现谷歌服务直连。要添加规则，只需在dnsmasq.conf文件里追加内容即可。

DNSMASQ的泛解析规则是这样的：

    address=/baidu.com/1.1.1.1

这意味着，*.baidu.com/*都将被引导至IP为1.1.1.1。

### Google 服务泛解析

下面来添加适用于谷歌大多数服务的泛解析规则。

首先需要寻找Google一个可用的IP。它最好是位于美国的服务器，这样能保证大多数服务可用。

已知Google现在一个可用的IP是74.125. 204.166。（注意，此 IP 在您现在阅读的这段时间并不一定有效）

追加规则：

    address=/google.com/74.125.204.166

    address=/googleapis.com/74.125.204.166

    address=/googlevideo.com/74.125.204.166

    address=/google.com.hk/74.125.204.166

    address=/youtube.com/74.125.204.166

    address=/ytimg.com/74.125.204.166

    address=/ggpht.com/74.125.204.166

    address=/googleusercontent.com/74.125.204.166

这基本上涵盖了绝大多数谷歌服务。把这些规则追加到dnsmasq.conf文件里即可。

### Wikipedia 服务泛解析

下面提供一组维基百科的泛解析规则：

    address=/wikipedia.org/91.198.174.192

    address=/wikimedia.org/208.80.154.224

    address=/upload.wikimedia.org/198.35.26.112

### 重启dnsmasq来应用

在修改之后你不会立即看到效果，因为有缓存效果。

在终端运行：

    sudo launchctl stop homebrew.mxcl.dnsmasq

    sudo launchctl start homebrew.mxcl.dnsmasq

    sudo killall -HUP mDNSResponder

即可刷新缓存并重新启动dnsmasq服务。

Chrome用户还可以在`Chrome://net-internals/#dns`清理缓存。
