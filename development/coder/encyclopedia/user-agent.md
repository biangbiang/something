ua浏览器之常见浏览器的UA值
=========================

UA值就是User Agent的简称,它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等。

一些网站常常通过判断 UA 来给不同的操作系统、不同的浏览器发送不同的页面，因此可能造成某些页面无法在某个浏览器中正常显示，但通过伪装 UA 可以绕过检测。

那么修改UA值的方法一般大家都会使用 Firefox 扩展 User Agent Switcher 来将浏览器的 User Agent 修改的五花八门，其实不使用扩展也可以修改 Firefox 的 UA：

在 Firefox 地址栏中输入 about:config。

右键新建一个名为 general.useragent.override 的 String 键值。（要是该键值已经存在就不用新建了。）将这个键值赋值为你想要修改的 UA。

所以我在这里放上一些常见的浏览器的UA值，好让大家修改浏览器的UA值。

    IE6.0:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
    IE7.0:Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)
    IE8.0:Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)
    IE9.0:Mozilla/4.0 (compatible; MSIE 9.0; Windows NT 6.1)
    Firefox3.5:Mozilla/5.0 (compatible; rv:1.9.1) Gecko/20090702 Firefox/3.5
    Firefox3.6:Mozilla/5.0 (compatible; rv:1.9.2) Gecko/20100101 Firefox/3.6
    Firefox4.0:Mozilla/5.0 (compatible; rv:2.0) Gecko/20110101 Firefox/4.0
    Firefox6.0:Mozilla/5.0 (Windows NT 6.1; WOW64; rv:6.0.2) Gecko/20100101 Firefox/6.0.2
    Chrome11.0:Mozilla/5.0 (compatible) AppleWebKit/534.21 (KHTML, like Gecko) Chrome/11.0.682.0 Safari/534.21
    Opera11.0:Opera/9.80 (compatible; U) Presto/2.7.39 Version/11.00
    Maxthon3.0:Mozilla/5.0 (compatible; U) AppleWebKit/533.1 (KHTML, like Gecko) Maxthon/3.0.8.2 Safari/533.1
    Android:Mozilla/5.0 (Linux; U; Android 2.2) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1
    iPhone:Mozilla/5.0 (iPhone; U; CPU OS 4_2_1 like Mac OS X) AppleWebKit/532.9 (KHTML, like Gecko) Version/5.0.3 Mobile/8B5097d Safari/6531.22.7
    iPad:Mozilla/5.0 (iPad; U; CPU OS 4_2_1 like Mac OS X) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/4.0.2 Mobile/8C148 Safari/6533.18.5
    Safari5.0:Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_7) AppleWebKit/534.16+ (KHTML, like Gecko) Version/5.0.3 Safari/533.19.4
