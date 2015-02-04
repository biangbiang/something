微信支付
========

### 获取微信版本号

由于微信5.0版本后才加入微信支付模块，低版本用户调用微信支付功能将无效。因此，建议商户通过user agent来确定用户当前的版本号后再调用支付接口。以iPhone版本为例，可以通过user agent可获取如下微信版本示例信息：

    "Mozilla/5.0(iphone;CPU iphone OS 5_1_1 like Mac OS X) AppleWebKit/534.46(KHTML,like Geocko) Mobile/9B206 MicroMessenger/5.0"

其中5.0为用户安装的微信版本号，商户可以解析以上HTTP头，获取到微信版本号是否高于或者等于5.0。
