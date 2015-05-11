RMI/XML-RPC/JSON-RPC/SOAP概念比较
=================================

### RMI

RMI ：Romote Method  Invocation，远程方法调用。基于java远程消息交换协议JRMP通信；JRMP是专为java远程对象制定的协议。是分布式应用程序的100%java解决方法。RMI对非java语言应用程序支持不足，不能实现互通。

RMI是面向对象的编程模型。广泛应用与EJB架构系统中。

RMI基于调用 的模式，调用过程如下：客户端程序调用服务对象的客户端代理，代理负责打包参数并通过JRMP协议发送到服务端，服务端使用同样协议解析，执行业务逻辑处理，用同样方法返回结果给客户端。
 
### RPC

RPC ：RPC算是这几类的统称（这样说有点不准确，但也可以这么理解）。  RPC（Remote Procedure Call）远程过程调用，是实现分布式计算的一种技术。在某种传输协议（TCP/HTTP等)上携带信息数据，通过网络从远程计算机程序上请求服务。在 OSI模型中，RPC跨越了传输层和应用层，使开发网络分布式应用程序变得容易。客户端代码像调用本地方法一样调用远程方法。

RPC基于请求应答 模式，客户端发送调用信息（将远程方法名、参数打包进请求信息）到服务端，服务端解析到要调用的对象和方法执行后返回应答信息；客户端接受相应获取应答信息。

RPC是跨语言的通信标准，sun和微软都有其实现,微软的DCOM就是建立在ORPC协议之上。

RPC是面向过程的编程模型。
 
### XML-RPC

XML-RPC :XML Remote Procedure Call，即XML远程方法调用,利用http+xml封装进行RPC调用。基于http协议传输、XML作为信息编码格式。一个xml-rpc消息就是 一个请求体为xml的http-post请求，服务端执行后也以xml格式编码返回。这个标准面前已经演变为下面的SOAP协议。可以理解SOAP是 XML-RPC的高级版本。

### SOAP

SOAP :Simple Object Access Protocol ,简单对象访问协议，是一种轻量的、简单的、基于xml的远程访问协议。可以与现有的多种传输层或应用层协议结合使用，如TCP、HTTP、SMTP等。 SOAP广泛使用的是基于HTTP和xml协议的实现（SOAP=RPC+HTTP+XML ），也就是大家常提的Web Service使用的通信协议。一个SOAP方法可以简单地看作遵循SOAP编码规则的HTTP请求和响应。

比较：XML－RPC是启动web服务最容易的方法，在很多方面比SOAP更简单易用，但不同于SOAP的是，XML－RPC没有相应的服务描述语法，这妨碍了XML－RPC服务的自动调用。

### JSON-RPC

JSON-RPC :JSON Remote Procedure Call，即JSON远程方法调用 。类似于XML-RPC，不同之处是使用JSON作为信息交换格式。

### 关于sun java版本的一些web服务规范：

JAX-RPC1.1:Java API for XML-Based RPC 1.1 

JAX-WS2.0:Java API for XML Web Service 2.0，是前者的升级版本。

使用JAXB处理xml与java对象映射；xml解析使用StAX拉式解析器规范。

详细可参考  Web 服务提示与技巧: JAX-RPC 与 JAX-WS 的比较

JAX-RS： (Java API for RESTful Web Services (JSR-311) )  

Java 上构建 RESTful 风格的 web services 提供的一组标准 API。
