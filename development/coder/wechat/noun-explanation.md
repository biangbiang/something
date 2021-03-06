名词解释
=======

1. 微信公众平台

  微信公众平台是微信公众账号申请入口和管理后台。商户可以在公众平台提交基本资料、业务资料、财务资料申请开通微信支付功能。

  平台入口：http://mp.weixin.qq.com。

2. 微信开放平台

  微信开放平台是商户APP接入微信支付开放接口的申请入口，通过此平台可申请微信APP支付。

  平台入口：http://open.weixin.qq.com。

3. 微信商户平台

  微信商户平台是微信支付相关的商户功能集合，包括参数配置、支付数据查询与统计、在线退款、代金券或立减优惠运营等功能。

  平台入口：http://pay.weixin.qq.com。

4. 微信支付系统

  微信支付系统是指完成微信支付流程中涉及的API接口、后台业务处理系统、账务系统、回调通知等系统的总称。

5. 商户收银系统

  商户收银系统即商户的POS收银系统，是录入商品信息、生成订单、客户支付、打印小票等功能的系统。接入微信支付功能主要涉及到POS软件系统的开发和测试，所以在下文中提到的商户收银系统特指POS收银软件系统。

6. 商户后台系统

  商户后台系统是商户后台处理业务系统的总称，例如：商户网站、收银系统、进销存系统、发货系统、客服系统等。

7. 扫码设备

  一种输入设备，主要用于商户系统快速读取媒介上的图形编码信息。按读取码的类型不同，可分为条码扫码设备和二维码扫码设备。按读取物理原理可分为红外扫码设备、激光扫码设备。

8. 商户证书

  商户证书是微信提供的二进制文件，商户系统发起与微信支付后台服务器通信请求的时候，作为微信支付后台识别商户真实身份的凭据。

9. 签名

  商户后台和微信支付后台根据相同的密钥和算法生成一个结果，用于校验双方身份合法性。签名的算法由微信支付制定并公开，常用的签名方式有：MD5、SHA1、SHA256、HMAC等。

10. JSAPI网页支付

  即前文说的微信内网页支付，可在微信公众号、朋友圈、聊天会话中点击页面链接，或者用微信“扫一扫”扫描页面地址二维码在微信中打开商户HTML5页面，在页面内下单完成支付。

11. Native原生支付

  Native原生支付属于扫码支付，商户根据微信支付协议格式生成的二维码，用户通过微信“扫一扫”扫描二维码后即进入付款确认界面，输入密码即完成支付。

12. 支付密码

  支付密码是用户开通微信支付时单独设置的密码，用于确认支付完成交易授权。该密码与微信登录密码不同。

13. Openid

  用户在公众号内的身份标识，不同公众号拥有不同的openid。商户后台系统通过登录授权、支付通知、查询订单等API可获取到用户的openid。主要用途是判断同一个用户，对用户发送客服消息、模版消息等。
