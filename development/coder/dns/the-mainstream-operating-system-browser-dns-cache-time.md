# 主流操作系统、浏览器DNS缓存时间

## 1 操作系统的DNS缓存
 
### 1.1 windows

windows DNS缓存的默认值是 MaxCacheTTL，见这里，它的默认值是86400s，也就是一天。它是TTLu 这篇文章列出了一些浏览器的DNS缓存时间。

浏览器DNS缓存的时间跟ttl值无关，每种浏览器都使用一个固定值。

### 1.2 macOS

macOS 严格遵循DNS协议中的TTL

## 2 浏览器的DNS缓存

浏览器为了提高响应时间，也会缓存DNS记录。

这篇文章 列出了一些浏览器的DNS缓存时间

浏览器DNS缓存时间跟TTL无关，每种浏览器都有一个固定值

### 2.1 chrome

为了加快访问速度，Google Chrome浏览器采用了预提DNS记录，在本地建立DNS缓存的方法，加快网站的连接速度。

chrome://net-internals/#dns 这里可以看各域名的DNS 缓存时间。chrome对每个域名会默认缓存60s。

### 2.2 IE

IE将DNS缓存30min。见这里

### 2.3 firefox

Firefox有dns缓存功能，但是默认缓存时间只有1分钟，可以通过修改该默认值加快DNS解析速度，方法如下：

打开一个新的窗口，地址栏输 入 about:config，回车，进入设置界面。然后搜索 network.dnsCacheExpiration ，把原来的60改成 6000（表示缓存6000秒），再搜索network.dnsCacheEntries 把默认的20改成1000（表示缓存1000条）。如果没 有上面两个项目，新建它们即可，

新建条目类型为整数型。 当然也可以按照需要设置成其它的值。

### 2.4 safari

约为10s

## 3 浏览器对DNS解析结果的处理

如果一个域名的DNS解析结果会有多个的话，浏览器是如何处理的呢？

Chrome浏览器会优先向第一个IP发起HTTP请求，如果不通，再向后面的IP发起HTTP请求。

    Date: 2014-11-21T17:13+0800

    Author: CobbLiu

    Org version 7.9.3f with Emacs version 24
