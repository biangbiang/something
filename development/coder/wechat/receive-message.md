接收消息
=======

当普通微信用户向公众账号发消息时，微信服务器将POST消息的XML数据包到开发者填写的URL上。各消息类型的推送XML数据包结构如下：

微信服务器在五秒内收不到响应会断掉连接，并且重新发起请求，总共重试三次

关于重试的消息排重，推荐使用msgid排重。

假如服务器无法保证在五秒内处理并回复，可以直接回复空串，微信服务器不会对此作任何处理，并且不会发起重试。

### 目录

1. 文本消息
2. 图片消息
3. 语音消息
4. 视频消息
5. 地理位置消息
6. 链接消息

### 文本消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName> 
    <CreateTime>1348831860</CreateTime>
    <MsgType><![CDATA[text]]></MsgType>
    <Content><![CDATA[this is a test]]></Content>
    <MsgId>1234567890123456</MsgId>
    </xml>

    参数	描述
    ToUserName	开发者微信号
    FromUserName	发送方帐号（一个OpenID）
    CreateTime	消息创建时间 （整型）
    MsgType	text
    Content	文本消息内容
    MsgId	消息id，64位整型

### 图片消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1348831860</CreateTime>
    <MsgType><![CDATA[image]]></MsgType>
    <PicUrl><![CDATA[this is a url]]></PicUrl>
    <MediaId><![CDATA[media_id]]></MediaId>
    <MsgId>1234567890123456</MsgId>
    </xml>
    
    参数	描述
    ToUserName	开发者微信号
    FromUserName	发送方帐号（一个OpenID）
    CreateTime	消息创建时间 （整型）
    MsgType	image
    PicUrl	图片链接
    MediaId	图片消息媒体id，可以调用多媒体文件下载接口拉取数据。
    MsgId	消息id，64位整型

### 语音消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1357290913</CreateTime>
    <MsgType><![CDATA[voice]]></MsgType>
    <MediaId><![CDATA[media_id]]></MediaId>
    <Format><![CDATA[Format]]></Format>
    <MsgId>1234567890123456</MsgId>
    </xml>

    参数	描述
    ToUserName	开发者微信号
    FromUserName	发送方帐号（一个OpenID）
    CreateTime	消息创建时间 （整型）
    MsgType	语音为voice
    MediaId	语音消息媒体id，可以调用多媒体文件下载接口拉取数据。
    Format	语音格式，如amr，speex等
    MsgID	消息id，64位整型

### 视频消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1357290913</CreateTime>
    <MsgType><![CDATA[video]]></MsgType>
    <MediaId><![CDATA[media_id]]></MediaId>
    <ThumbMediaId><![CDATA[thumb_media_id]]></ThumbMediaId>
    <MsgId>1234567890123456</MsgId>
    </xml>

    参数	描述
    ToUserName	开发者微信号
    FromUserName	发送方帐号（一个OpenID）
    CreateTime	消息创建时间 （整型）
    MsgType	视频为video
    MediaId	视频消息媒体id，可以调用多媒体文件下载接口拉取数据。
    ThumbMediaId	视频消息缩略图的媒体id，可以调用多媒体文件下载接口拉取数据。
    MsgId	消息id，64位整型

### 地理位置消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1351776360</CreateTime>
    <MsgType><![CDATA[location]]></MsgType>
    <Location_X>23.134521</Location_X>
    <Location_Y>113.358803</Location_Y>
    <Scale>20</Scale>
    <Label><![CDATA[位置信息]]></Label>
    <MsgId>1234567890123456</MsgId>
    </xml> 

    参数	描述
    ToUserName	开发者微信号
    FromUserName	发送方帐号（一个OpenID）
    CreateTime	消息创建时间 （整型）
    MsgType	location
    Location_X	地理位置维度
    Location_Y	地理位置经度
    Scale	地图缩放大小
    Label	地理位置信息
    MsgId	消息id，64位整型

### 链接消息

    <xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1351776360</CreateTime>
    <MsgType><![CDATA[link]]></MsgType>
    <Title><![CDATA[公众平台官网链接]]></Title>
    <Description><![CDATA[公众平台官网链接]]></Description>
    <Url><![CDATA[url]]></Url>
    <MsgId>1234567890123456</MsgId>
    </xml> 

    参数	描述
    ToUserName	接收方微信号
    FromUserName	发送方微信号，若为普通用户，则是一个OpenID
    CreateTime	消息创建时间
    MsgType	消息类型，link
    Title	消息标题
    Description	消息描述
    Url	消息链接
    MsgId	消息id，64位整型
