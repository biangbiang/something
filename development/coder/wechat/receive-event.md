接收事件推送
===========

### 目录

	1. 关注/取消关注事件
	2. 扫描带参数二维码事件
	3. 上报地理位置事件
	4. 自定义菜单事件
	5. 点击菜单拉取消息时的事件推送
	6. 点击菜单跳转链接时的事件推送

### 关注/取消关注事件

用户在关注与取消关注公众号时，微信会把这个事件推送到开发者填写的URL。方便开发者给用户下发欢迎消息或者做帐号的解绑。

微信服务器在五秒内收不到响应会断掉连接，并且重新发起请求，总共重试三次

关于重试的消息排重，推荐使用FromUserName + CreateTime 排重。

假如服务器无法保证在五秒内处理并回复，可以直接回复空串，微信服务器不会对此作任何处理，并且不会发起重试。

推送XML数据包示例：

	<xml>
	<ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[FromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[subscribe]]></Event>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，subscribe(订阅)、unsubscribe(取消订阅)

### 扫描带参数二维码事件

用户扫描带场景值二维码时，可能推送以下两种事件：

1. 如果用户还未关注公众号，则用户可以关注公众号，关注后微信会将带场景值关注事件推送给开发者。
2. 如果用户已经关注公众号，则微信会将带场景值扫描事件推送给开发者。

#### 1. 用户未关注时，进行关注后的事件推送

推送XML数据包示例：

	<xml><ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[FromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[subscribe]]></Event>
	<EventKey><![CDATA[qrscene_123123]]></EventKey>
	<Ticket><![CDATA[TICKET]]></Ticket>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，subscribe
	EventKey	事件KEY值，qrscene_为前缀，后面为二维码的参数值
	Ticket	二维码的ticket，可用来换取二维码图片

#### 2. 用户已关注时的事件推送

推送XML数据包示例：

	<xml>
	<ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[FromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[SCAN]]></Event>
	<EventKey><![CDATA[SCENE_VALUE]]></EventKey>
	<Ticket><![CDATA[TICKET]]></Ticket>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，SCAN
	EventKey	事件KEY值，是一个32位无符号整数，即创建二维码时的二维码scene_id
	Ticket	二维码的ticket，可用来换取二维码图片

### 上报地理位置事件

用户同意上报地理位置后，每次进入公众号会话时，都会在进入时上报地理位置，或在进入会话后每5秒上报一次地理位置，公众号可以在公众平台网站中修改以上设置。上报地理位置时，微信会将上报地理位置事件推送到开发者填写的URL。

推送XML数据包示例：

	<xml>
	<ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[fromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[LOCATION]]></Event>
	<Latitude>23.137466</Latitude>
	<Longitude>113.352425</Longitude>
	<Precision>119.385040</Precision>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，LOCATION
	Latitude	地理位置纬度
	Longitude	地理位置经度
	Precision	地理位置精度

### 自定义菜单事件

用户点击自定义菜单后，微信会把点击事件推送给开发者，请注意，点击菜单弹出子菜单，不会产生上报。

#### 点击菜单拉取消息时的事件推送

推送XML数据包示例：

	<xml>
	<ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[FromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[CLICK]]></Event>
	<EventKey><![CDATA[EVENTKEY]]></EventKey>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，CLICK
	EventKey	事件KEY值，与自定义菜单接口中KEY值对应

#### 点击菜单跳转链接时的事件推送

推送XML数据包示例：

	<xml>
	<ToUserName><![CDATA[toUser]]></ToUserName>
	<FromUserName><![CDATA[FromUser]]></FromUserName>
	<CreateTime>123456789</CreateTime>
	<MsgType><![CDATA[event]]></MsgType>
	<Event><![CDATA[VIEW]]></Event>
	<EventKey><![CDATA[www.qq.com]]></EventKey>
	</xml>

参数说明：

	参数	描述
	ToUserName	开发者微信号
	FromUserName	发送方帐号（一个OpenID）
	CreateTime	消息创建时间 （整型）
	MsgType	消息类型，event
	Event	事件类型，VIEW
	EventKey	事件KEY值，设置的跳转URL
