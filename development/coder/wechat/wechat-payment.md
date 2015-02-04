微信支付
========

## 微信内网页支付

### 获取微信版本号

由于微信5.0版本后才加入微信支付模块，低版本用户调用微信支付功能将无效。因此，建议商户通过user agent来确定用户当前的版本号后再调用支付接口。以iPhone版本为例，可以通过user agent可获取如下微信版本示例信息：

    "Mozilla/5.0(iphone;CPU iphone OS 5_1_1 like Mac OS X) AppleWebKit/534.46(KHTML,like Geocko) Mobile/9B206 MicroMessenger/5.0"

其中5.0为用户安装的微信版本号，商户可以解析以上HTTP头，获取到微信版本号是否高于或者等于5.0。

### 显示微信安全支付标题

对于商户具有支付权限且需要调用微信支付的页面，为了让用户增加购买信心，确认交易环境安全，微信强烈建议商户使用“微信安全支付”标题。安全支付标题的如下图:

![](http://biangbiangpic.b0.upaiyun.com/blog/2b0a3a03805dd6acd3f689e6864715be.png)

图-微信安全支付标题

显示支付安全标题，需将原始链接添加上"showwxpaytitle=1"的尾串。通过这种方式，商户的页面将出现微信安全支付的标识。

例如，原始URL为：http://weixin.qq.com,显示安全支付标题的URL为：http://weixin.qq.com?showwxpaytitle=1。

当用户在微信里打开http://weixin.qq.com不会直接出现微信安全支付的标题，而打开http://weixin.qq.com?showwxpaytitle=1后将出现微信安全支付标题。

### 业务流程时序图

![](http://biangbiangpic.b0.upaiyun.com/blog/e7e97ea60c072d857def887e785ec2a2.png)

图-微信内网页支付时序图
