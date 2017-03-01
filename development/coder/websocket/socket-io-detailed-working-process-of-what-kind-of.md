# socket.io 的详细工作流程是怎样的

http://socket.io 是基于 WebSocket 的 C-S 实时通信库，我假设题目问的是 http://socket.io 而非 WebSocket 协议的实现。

http://socket.io 底层是 http://engine.io，这个库实现了跨平台的双向通信。

http://engine.io 使用了 WebSocket 和 XMLHttprequest（或JSONP） 封装了一套自己的 Socket 协议（暂时叫 EIO Socket），在低版本浏览器里面使用长轮询替代 WebSocket。一个完整的 EIO Socket 包括多个 XHR 和 WebSocket 连接.

### 前端：

EIO Socket 通过一个 XHR (XMLHttprequest) 握手。前端发送一个 XHR，告诉服务端我要开始 XHR 长轮询了。后端返回的数据里面包括一个 open 标志(数字 0 表示), 以及一个 sid 和 upgrades 字段。

sid 是本次 EIO Socket 的会话 ID，因为一次 EIO Socket 包含了多个请求，而后端又会同时连接多个 EIO Socket，sid 的作用就相当于 SESSION ID。

另一个字段 upgrades，正常情况下是 ['websocket']，表示可以把连接方式从长轮询升级到 WebSocket.

前端在发送第一个 XHR 的时候就开始了 XHR 长轮询，这个时候如果有收发数据的需求，是通过长轮询实现的。所谓长轮询，是指前端发送一个 request，服务端会等到有数据需要返回时再 response. 前端收到 response 后马上发送下一次 request。这样就可以实现双向通信。

前端收到握手的 upgrades 后，EIO 会检测浏览器是否支持 WebSocket，如果支持，就会启动一个 WebSocket 连接，然后通过这个 WebSocket 往服务器发一条内容为 probe, 类型为 ping 的数据。如果这时服务器返回了内容为 probe, 类型为 pong 的数据，前端就会把前面建立的 HTTP 长轮询停掉，后面只使用 WebSocket 通道进行收发数据。

EIO Socket 生命周期内，会间隔一段时间 ping - pong 一次，用来测试网络是否正常。

![](http://biangbiangpic.b0.upaiyun.com/blog/0ebd7ff26896a27e15386d4bc07bfe3d.png)

这是 WebSocket 帧的结构，绿色是发送，白色是接收。前面的数字是数据包类型，2 是 ping, 3 是 pong, 4 是 message.

http://socket.io 在 http://engine.io 的基础上做了一些封装，比如 http://socket.io 里面这样的代码：

    io.emit('add user', 'm')

在 engine.io 里面是这样：

    eio.send('message', '2["add user","m"]') // 2 是 socket.io 定义的包类型

再例如，engine.io-client 需要在 open 之后才能 send，而 http://socket.io 就不需要，open 之前 emit 的数据会在 open 之后再发出。

另外 http://socket.io 还提供了 namespace，复用, 自动重连等特性。

### 服务端：

服务端使用 ws 库实现 WebSocket 协议。

http://socket.io 服务启动时，会先启动一个 ws 服务。http://socket.io 会监听 HTTP 服务器的 upgrade 和 request 事件。

当 upgrade 事件触发时，说明可能是 WebSocket 握手，先简单校验下，然后把请求交给 ws 服务进行处理，拿到 WebSocket 对象。

当 request 事件触发时，根据 url 路径判断是不是 http://socket.io 的 XHR 请求，拿到 res 和 res 对象。

这样就可以正确接收和返回客户端数据了，具体处理过程和前端部分是对应的。
