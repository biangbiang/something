# 用代理自动配置文件pac给iphone和ipad设备添加socks代理

iOS实际上支持socks代理的，但在 iPhone和iPad设备的

Setting -> WLAN 下只能看到HTTP Proxy

HTTP Prxoy有３种模式: 关闭Off/手动Manual/自动Auto

用自动配置文件，就可以支持socks代理

    function FindProxyForURL(url, host)
    {
         return "SOCKS proxy_host:proxy_port";
    }

将上面的`proxy_host`和`proxy_port`换成socks服务器实际的IP地址和端口

然后保存为　proxy.pac

放到　http://biang.io/proxy.pac

在 iPhone设备中，添加自动配置 URL 为上面的地址，就可以使用socks代理了

### 扩展1:

局域网流量不要用代理

    function FindProxyForURL(url, host)
    {
        if (isInNet(host, "192.168.1.0", "255.255.255.0"))
            return "DIRECT";

        return "SOCKS proxy_host:proxy_port";
    }

参考资料　　[代理自动配置（Proxy Auto Config)](http://zh.wikipedia.org/zh/%E4%BB%A3%E7%90%86%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE)

一个PAC文件包含一个JavaScript的函数”FindProxyForURL(url, host)”. 这个函数返回一个包含一个或多个访问规则的字符串。用户代理根据这些规则适用一个特定的代理其或者直接访问。 当一个代理服务器无法响应的时候，多个访问规则提供了其他的后备访问方法。 浏览器在访问其他页面以前，首先访问这个PAC文件。PAC文件中的URL可能是手工配置的，也可能是是通过网页的网络代理自发现协议（Web Proxy Autodiscovery Protocol)自动配置的。

为了完整性和最佳的兼容性，我们应该设置网页服务器(apache或者nginx,lighttpd等等)将这个pac文件的MIME类型声明为 application/x-ns-proxy-autoconfig 或者 application/x-javascript-config

对于现代浏览器来说，两种mime类型都没有问题，　但 application/x-ns-proxy-autoconfig 相对 application/x-javascript-config 被更多的客户端所支持,　因为它被定义在最初的Netscape规范里

FindProxyForURL的返回值，可以是以下３种之一，或者是它们的组合

    DIRECT
       直接连接，不使用代理

    PROXY host:port
    　　　使用指定的http代理

    SOCKS host:port
    　　　使用指定的SOCKS代理

如果是多个组合，要用　分号;拼接起来

并且，放在最左边的优先级最高，如果最左边的失败，依次往右边尝试

如果最左边的代理服务器挂掉了，并且你在右边没有指定DIRECT选项，　浏览器应该会问你

代理服务器挂了，能不能临时忽略它，直接连接网络?

参考资料　　

[PAC Functions](http://findproxyforurl.com/pac-functions/)　（强烈推荐)

[Navigator Proxy Auto-Config File Format](http://linuxmafia.com/faq/Web/autoproxy.html) (NetScape原始规范)

[The Practical Proxy PAC file guid](http://www.proxypacfiles.com/proxypac/) (最最强烈推荐)

    shExpMatch(str, shexp)
    第1个参数str, 待比较的任意字符串，一般是url或者host
    第2个参数shexp是shell表达式,随意可以有*号通配符

    isPlainHostName(host)
    如果host是不含有”.”,　说明是一个主机名， 就返回true;
    如果有点号，说明是一个域名, 返回false
    host是从URL中分离出来的主机名（不包括端口号)

    isInNet(host_or_ip, pattern, mask)
    这个函数将会考察第1个参数 ip地址 (如果传入的参数是主机名，会被自动转换为ip地址)
    看它是否在　第2个参数pattern 和　第3个参数　mask 决定的子网络范围内

例如

    if (isInNet(dnsResolve(host), "172.16.0.0", "255.240.0.0"))
        return "DIRECT";

    if (isInNet(myIpAddress(), "10.10.1.0", "255.255.255.0"))
        return "PROXY 10.10.5.1:8080";

其中myIpAddress() 返回　浏览器所在的主机的当前IP地址，　很多时候这是个局域网地址

dnsResolve(host) 将host解析为ip地址

    var resolved_ip = dnsResolve(host);
    if (isInNet(dnsResolve(host), "10.0.0.0", "255.0.0.0") ||
        isInNet(dnsResolve(host), "172.16.0.0",  "255.240.0.0") ||
        isInNet(dnsResolve(host), "192.168.0.0", "255.255.0.0") ||
        isInNet(dnsResolve(host), "127.0.0.0", "255.255.255.0"))
        return "DIRECT";

这段必须用上啊

    dnsDomainIs(host, domain)

    if (dnsDomainIs(host, ".google.com"))
        return "DIRECT";

其中host是从URL分离出来的主机名

    localHostOrDomainIs(host, hostdom)

    if (localHostOrDomainIs(host, "www.google.com"))
        return "DIRECT";

hostdom 是　域名全称

host 既可以是　域名，也可以是　简短主机名

如果是域名，必须完全同　hostdom 匹配

如果是主机名，必须同　hostdom的主机名　要完全匹配

isResolvable(host)

dnsResolve(host)

dnsDomainLevels(host)

dnsDomainLevels("www")

返回 0

dnsDomainLevels("www.netscape.com")

返回 2

### 调试是个大问题

PAC是一个javascript脚本，浏览器在每次请求一个URL之前，都会运行它．

但它和普通的js脚本有点不同

1)在PAC中，有个几个特殊的函数，它只能在PAC运行，在普通的js脚本运行会报告

“is not defined”

比如　shExpMatch

2)PAC的执行，也不支持js的所有特性．

3)在不同的浏览器中，对pac脚本的执行有着不同的实现方式．

可以用Firefox的错误控制台来查看错误信息

IE也支持alert()

并且　pac文件可以存在于本地文件系统

用　file:///var/run/x.pac 这样的路径

而不要用　http ,方便调试

     function FindProxyForURL(url, host)
        {
            if (isPlainHostName(host) || dnsDomainIs(host, ".mydomain.com"))
                return "DIRECT";
            else if (shExpMatch(host, "*.com"))
                return "PROXY proxy1.mydomain.com:8080; " +
                       "PROXY proxy4.mydomain.com:8080";
            else if (shExpMatch(host, "*.edu"))
                return "PROXY proxy2.mydomain.com:8080; " +
                       "PROXY proxy4.mydomain.com:8080";
            else
                return "PROXY proxy3.mydomain.com:8080; " +
                       "PROXY proxy4.mydomain.com:8080";
        }

我的最终版本

    function FindProxyForURL(url, host)
    {
        url  = url.toLowerCase();
        host = host.toLowerCase();

        if (isInNet(dnsResolve(host), "10.0.0.0", "255.0.0.0") ||
            isInNet(dnsResolve(host), "172.16.0.0",  "255.240.0.0") ||
            isInNet(dnsResolve(host), "192.168.0.0", "255.255.0.0") ||
            isInNet(dnsResolve(host), "127.0.0.0", "255.255.255.0"))
            return "DIRECT";

        if (shExpMatch(url,"*twitter*")  ||
            shExpMatch(url,"*facebook*") ||
            shExpMatch(url,"*blogspot*") ||
            shExpMatch(url,"*youtube*") ||
           )
        {
           return "SOCKS 1.2.3.4:1080; DIRECT";
        }
    }

Chrome在Linux下没有代理配置界面，但可以通过命令行参数配置

    --proxy-server=host:port
    --no-proxy-server
    --proxy-auto-detect
    --proxy-pac-url=URL

pac就用

    --proxy-pac-url=file:///var/run/autoproxy.pac

新的浏览器支持正则表达式

如果需要做一些较为复杂的判断，那可直接抛弃 shExpMatch 函数，而自己使用正则表达式或别的工具来进行判断，如：

     var regexpr = /[a-zA-Z]{4}.microsoft.com/;
        if(regexpr.test(host))
            return "PROXY w3proxy:8080; DIRECT";

调试，使用alert,在IE上没问题

    function isMatchProxy(url, pattern) {
        try {
            return new RegExp(pattern.replace('.', '.')).test(url);
        } catch (e) {
            return false;
        }
    }

    function FindProxyForURL(url, host) {
        debugPAC ="PAC Debug Informationn";
        debugPAC +="-----------------------------------n";
        debugPAC +="Machine IP: " + myIpAddress() + "n";
        debugPAC +="Hostname: " + host + "n";
        if (isResolvable(host)) {
            resolvableHost = "True"
        } else {
            resolvableHost = "False"
        };
        debugPAC += "Host Resolvable: " + resolvableHost + "n";
        debugPAC += "Hostname IP: " + dnsResolve(host) + "n";
        if (isPlainHostName(host)) {
            plainHost = "True"
        } else {
            plainHost = "False"
        };
        debugPAC += "Plain Hostname: " + plainHost + "n";
        debugPAC += "Domain Levels: " + dnsDomainLevels(host) + "n";
        debugPAC += "URL: " + url + "n";
        alert(debugPAC);
        var Proxy = 'SOCKS 1.2.3.4:9625; DIRECT';

        var list = [
            't.co',
            'twitter.com',
            'twimg.com',
            'posterous.com',
            'tinypic.com',
            'twitpic.com',
            'bitly.com',
            'yfrog.com',
            'youtube.com',
            'facebook.com',
            'appspot.com',
            'dropbox.com',
            'flickr.com',
            'youtube.com',
            'ytimg.com',
            'plus.google.com',
            'ggpht.com',
            'talkgadget.google.com',
            'picasaweb.google.com',
            'googleusercontent.com',
            'hzmangel.info',
            'slideshare.net',
            'code.google.com',
            'golang.org',
            'vimeo.com',
            'wordpress.com',
            'dxtl.net',
            '123cha.com'
        ];

        for(var i=0, l=list.length; i<l; i++) {
            if (isMatchProxy(url, list[i])) {
                alert("Match");
                return Proxy;
            }
        }

        alert("direct");
        return 'DIRECT';
    }

在PAC中，Firefox和Internet Explore都支持alert语句，IE的表现同普通js一样

Firefox是在　　”浏览器控制台”（Ctrl+Shift+J快捷键调出来) JS标签　里显示出来

    —
    [21:32:29.568] PAC-alert: PAC Debug Information
    ———————————–
    Machine IP: 192.168.1.99
    Hostname: zhiwei.li
    Host Resolvable: True
    Hostname IP: 199.188.204.95
    Plain Hostname: False
    Domain Levels: 1
    URL: http://zhiwei.li/

正则表达式的另外一个例子

由于.pac 文件支持整个 JavaScript 语言，可以使用正则表达式对象，并测试方法来测试对照正则表达式的字符串。下面的代码示例演示如何使用.pac 文件中的正则表达式对象：

    function FindProxyForURL(url, host)
    {
        // For instance, if the server has 4 alphabetic characters,
        // such as "MSDN", route it through a specific proxy:
       var regexpr = /[a-zA-Z]{4}.microsoft.com/;
       if(regexpr.test(host))
          return "PROXY w3proxy:8080; DIRECT";

       // Or else connect directly:
       return "DIRECT";
    }
    http://technet.microsoft.com/en-us/library/dd361950.aspx (微软给的例子)

验证工具

https://code.google.com/p/pacparser/　　　（支持Python和C)

http://www.jslint.com/ (验证你的js语法)

Debian Jessie中的IceWeasel 24.1.0 对 PAC的支持有问题，

Windows版本的Firefox 26.0就能很好的支持

解决方法，安装　扩炸　　foxyproxy

xul-ext-foxyproxy-standard 或者从mozilla addons下载

SOCKS SOCKS4 SOCKS5的问题

“SOCKS host:port”

有的浏览器使用SOCKS4协议，也支持DNS 解析　　（IceWeasel 的代理管理，就是这个做法，　但是SOCKS4实际上是不支持 DNS解析的，socks５服务看到协议版本是4,　dns请求是未知的，就会直接关掉连接

所以，　你在 firefox里选中socks4协议，就不要　让　extensions.foxyproxy.socks_remote_dns　这个选项为true

但是，如果不做远程dns的话，伟/大的-城＿墙　会污染DNS)

有的浏览器使用SOCKS4协议，但不支持 DNS解析

有的浏览器直接使用SOCKS5协议，当然就支持 DNS解析了 (foxyProxy扩展，看到 SOCKS，就直接用SOCKS5了,相当聪明)

“SOCKS5 host:port” 　　明确说明要用 SOCKS5 代理

据说　Safari (OSX, iOS)只认识SOCKS,虽然它默认也是使用SOCKS5协议

SOCKS5 127.0.0.1:1080; SOCKS 127.0.0.1:1080; DIRECT

这种写法可以兼容绝大数浏览器

对于不认识的SOCKS5,丢掉，认识的SOCKS直接用
