整理一些常用的DNS
=================

DNS是把方便用户记忆的域名转换成网络可辨别使用的IP地址，全称Domain Name System，即域名解析系统。在网络已经相当普及的今天，DNS劫持和污染也成为国内电信运营商干扰用户正常上网的手段，使得用户不得不忍受广告弹窗，并且一些常用的国外服务也无法正常访问。今天就介绍一些国内外常用的DNS服务器地址，供大家选择使用，尽早脱离DNS挟持和污染。

![](http://biang.io/biangpic/blog/e99cec17dde06bfd88e7eec35dcc2026.jpg)

先介绍一些国内的，毕竟离得近，解析上有着天然的优势。

一、114DNS（https://www.114dns.com/）

老牌的DNS解析服务了，以前经常用，电信联通移动，全国通用DNS，分的种类也较多，根据需要选择。 

纯净无劫持，无需再忍受被强扭去看广告或粗俗网站之痛苦：

    114.114.114.114 
    114.114.115.115

拦截钓鱼病毒木马网站，增强网银、证券、购物、游戏、隐私信息安全：

    114.114.114.119 
    114.114.115.119

学校或家长可选，拦截色情网站，保护少年儿童免受网络色情内容的毒害

    114.114.114.110 
    114.114.115.110

二、阿里DNS（http://www.alidns.com/）

阿里DNS是阿里巴巴集团推出的DNS递归解析系统，面向互联网用户提供快速、稳定、智能的免费DNS递归解析服务，现在我也经常使用。

    223.5.5.5 
    223.6.6.6

三、SDNS（http://www.sdns.cn/）

SDNS是由中国互联网络信息中心（CNNIC）与国内外电信运营商合作推出的免费公共云解析服务（SecureDNS，简称SDNS），旨在为用户提供高速、安全、智能的上网接入解析服务。

这两周使用114和阿里DNS都碰到了广告挟持的问题，换成SDNS后解决，正在使用。

    1.2.4.8 
    210.2.4.8

四、中科大的DNS

据一些网友反映：无污染、速度快。我是才知道，还没用过，不过考虑到中科大的教育性质和有名的Ubuntu源的速度，应该可用。

    202.38.64.1 
    202.112.20.131 
    202.141.160.95 
    202.141.160.99 
    202.141.176.95 
    202.141.176.99

五、OneDNS（http://www.onedns.net/）

创立于2013年的DNS服务，比较新，没使用过，不过他们的口号挺吸引人：OneDNS是一个安全、快速、免费的小众DNS服务。通过它，您可以时刻畅享来自云端的恶意网站屏蔽服务，彻底摆脱无良ISP的DNS污染与劫持，同时，横跨南北的高速线路加速您的网络连接。

    112.124.47.27 南方首选/北方备用 
    114.215.126.16 北方首选/南方备用

六、DNS派（http://www.dnspai.com/）

360的合作伙伴，口号：让网上冲浪更加稳定、快速、安全；为家庭拦截钓鱼网站，过滤非法网站，建立一个绿色健康的网上环境；为域名拼写自动纠错，让上网更方便。没使用过，不过看这口号没太大吸引力。

    电信：首选地址：101.226.4.6， 备用地址：218.30.118.6 
    联通：首选地址：123.125.81.6，备用地址：140.207.198.6 
    移动：首选地址：101.226.4.6， 备用地址：218.30.118.6 
    铁通：首选地址：101.226.4.6， 备用地址：218.30.118.6

七、HelloDNS（http://hellodns.org/）

看网站介绍后，立马就把DNS改过去试用下，顿时Google就好用了，就是Twitter和FaceBook的访问慢一些。这是网站介绍，你们感受下：HelloDNS支持edns-client-subnet，能够智能将请求发往最近、最适合线路的节点，实现更快速的互联网访问；同时HelloDNS能够有效解决DNS劫持，解析更精准，加速Google的各项服务、Facebook、Twitter等网站访问，实现Onedrive、Origin、AppStore 等服务的上传下载加速。

    123.56.46.123 
    121.40.144.82

    42.159.153.39

下面再列举几个国外的DNS，但由于地域的原因或者是天朝的限制 ，大家酌情选择吧，我现在是很少用了！

八、Google DNS（https://developers.google.com/speed/public-dns/）

说到DNS就绕不过去的槛，是Google于2009年12月5日起提供的一个免费域名解析服务。国外是王者风范，国内就残了一半。

    8.8.8.8 
    8.8.4.4

九、OpenDNS（https://www.opendns.com/）

    208.67.220.220 
    208.67.222.222

创建于2006年，国际大牌，解析功能稳定而全面，但在中国还是有点水土不服。

十、Norton DNS（https://dns.norton.com/）

Norton DNS是资深安全厂商诺顿针对家庭和企业用户推出的一项安全DNS服务。对我来说，是彻彻底底的备用，没事肯定不设他。它的功能也挺全面，将DNS地址分类三类安全等级，分别是:

A – 安全性（恶意软件，钓鱼网站和诈骗网站）

    199.85.126.10 
    199.85.127.10

B – 安全+色情

    199.85.126.20 
    199.85.127.20

C – 安全+色情+非家庭

    199.85.126.30 
    199.85.127.30

十一、OpenNIC DNS（http://www.opennicproject.org/）

OpenNIC是一个由热心的志愿者运行的非营利组织，主张DNS中立，为用户免费提供DNS服务，保障用户的DNS服务不被审查的自由。访问官网，会自动向你推荐最适合你所在区域的DNS。

![](http://biang.io/biangpic/blog/42921648328474e0fe2858aee62ddded.jpg)

十二、V2EX DNS（http://dns.v2ex.com/）

由V2EX站长提供，延迟有点高，偶尔掉包。之前为了提升App Store的访问速度时设置过，不是很稳定，解析的网站也不是很全面，偶尔为了特殊目的可以尝试下。

    199.91.73.222 
    178.79.131.110

好了，就写这几个吧，大部分需求应该都可以满足了。

最后，提醒一点：对于一些设置后可以访问敏感网站（如google）的公共DNS服务器，会将这些网站的IP解析到反向代理上，而这会对一些个人信息（如accounts.google.com）带来安全隐患，因此从安全角度来讲，对于这些网站，最好是通过修改Hosts文件的方式使用官方IP地址。
