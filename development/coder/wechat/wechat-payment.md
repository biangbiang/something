微信支付
========

## 支付模式

1. 被扫支付

  被扫支付是用户展示微信上“我的刷卡条码/二维码”给商户系统扫描后直接完成支付的模式。主要应用线下面对面收银的场景。

2. 扫码支付

  扫码支付是商户系统按微信支付协议生成支付二维码，用户再用微信“扫一扫”完成支付的模式。该模式适用于PC网站支付、实体店单品或订单支付、媒体广告支付等场景。

3. 微信内网页支付

  微信内网页支付是用户在微信中打开商户的H5页面，商户在H5页面通过调用微信支付提供的JSAPI接口调起微信支付模块完成支付。应用场景有：

  ◆　用户在微信公众账号内进入商家公众号，打开某个主页面，完成支付

  ◆　用户的好友在朋友圈、聊天窗口等分享商家页面连接，用户点击链接打开商家页面，完成支付

  ◆　将商户页面转换成二维码，用户扫描二维码后在微信浏览器中打开页面后完成支付


4. APP支付

  APP支付又称移动端支付，是商户通过在移动端应用APP中集成开放SDK调起微信支付模块完成支付的模式。

5. 普通浏览器网页支付模式

  正在建设中，敬请期待。

---

## 微信内网页支付

### 获取微信版本号

由于微信5.0版本后才加入微信支付模块，低版本用户调用微信支付功能将无效。因此，建议商户通过user agent来确定用户当前的版本号后再调用支付接口。以iPhone版本为例，可以通过user agent可获取如下微信版本示例信息：

    "Mozilla/5.0(iphone;CPU iphone OS 5_1_1 like Mac OS X) AppleWebKit/534.46(KHTML,like Geocko) Mobile/9B206 MicroMessenger/5.0"

其中5.0为用户安装的微信版本号，商户可以解析以上HTTP头，获取到微信版本号是否高于或者等于5.0。

### 显示微信安全支付标题

对于商户具有支付权限且需要调用微信支付的页面，为了让用户增加购买信心，确认交易环境安全，微信强烈建议商户使用“微信安全支付”标题。安全支付标题的如下图:

![](http://biang.io/biangpic/blog/2b0a3a03805dd6acd3f689e6864715be.png)

图-微信安全支付标题

显示支付安全标题，需将原始链接添加上"showwxpaytitle=1"的尾串。通过这种方式，商户的页面将出现微信安全支付的标识。

例如，原始URL为：http://weixin.qq.com,显示安全支付标题的URL为：http://weixin.qq.com?showwxpaytitle=1。

当用户在微信里打开http://weixin.qq.com不会直接出现微信安全支付的标题，而打开http://weixin.qq.com?showwxpaytitle=1后将出现微信安全支付标题。

### 业务流程时序图

![](http://biang.io/biangpic/blog/e7e97ea60c072d857def887e785ec2a2.png)

图-微信内网页支付时序图

商户系统和微信支付系统主要交互：

1. 商户server调用统一下单接口请求订单，api参见公共api【统一下单】

2. 商户server接收支付通知，api参见公共api【通用通知api】

3. 商户server查询支付结果，api参见公共api【查询订单api】
