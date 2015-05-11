web service与远程调用（RPC）的区别
==================================

web service顾名思义就是一个运行在web上的服务。这个服务通过网络为我们的程序提供服务方法。类似一个远程的服务提供者。

比如，一个提供天气预报的网站需要随时更新天气情况，在WEB上挂上一个随时问讯最新天气情况的服务。我们的程序就可以从这个服务上获取到当前最新的天气信息。

Web Service 是一个能够实现远程数据交互的一个技术和协议，通过HTML进行通讯。 他实现了 不同系统不同平台，不同开发语言和开发技术实现的软件系统之间的通讯。

### 根据例子运行后的感受到的区别：

Web service是一个运行在web上的服务。客户端不管是CS/BS，调用这个服务获得结果。

可以通过制作RPC的过程了解RPC的实质。

### 制作RPC的过程：

先做一个windows应用dll,然后“创建服务器”，使用Remoting命名空间，此“创建服务器”的项目可以是一个Console Application，最后制作“创建客户机”应用，那么服务器应用和客户机应用就可以通过网络来调用，但是服务要一直开着。简而言之：就是一个cs应用通过网络使用另外一个cs项目的方法。

### 其实现的原理并没有本质的区别，在应用开发层面上有以下区别：

1、Remoting可以灵活的定义其所基于的协议，如果定义为HTTP，则与Web Service就没有什么区别了，一般都喜欢定义为TCP，这样比Web Service稍为高效一些

2、Remoting不是标准，而Web Service是标准；

3、Remoting一般需要通过一个WinForm或是Windows服务进行启动，而Web Service则需要IIS进行启动。

4、在VS.net开发环境中，专门对Web Service的调用进行了封装，用起来比Remoting方便

我建议还是采用Web Service好些，对于开发来说更容易控制

Remoting一般用在C/S的系统中，Web Service是用在B/S系统中

后者还是各语言的通用接口

相同之处就是都基于XML

为了能清楚地描述Web Service 和Remoting之间得区别,我打算从他们的体系结构上来说起: 

### Web Service大体上分为5个层次: 

1. Http传输信道 

2. XML的数据格式 

3. SOAP封装格式 

4. WSDL的描述方式 

5. UDDI

### 总体上来讲，.NET 下的 Web Service结构比较简单，也比较容易理解和应用： 

一般来讲在.NET结构下的WebService应用都是基于.net framework以及IIS的架构之下，所以部署(Dispose)起来相对比较容易点. 

从实现的角度来讲，

首先WebService必须把暴露给客户端的方法所在的类继承于：System.Web.Services.WebService这个基类 

其次所暴露的方法前面必须有[WebMethod]或者[WebMethodAttribute]

### WebService的运行机理 

首先客户端从服务器的到WebService的WSDL，同时在客户端声称一个代理类(Proxy Class) 

这个代理类负责与WebService服务器进行Request 和Response 

当一个数据（XML格式的）被封装成SOAP格式的数据流发送到服务器端的时候，就会生成一个进程对象并且把接收到这个Request的SOAP包进行解析，然后对事物进行处理，处理结束以后再对这个计算结果进行SOAP包装，然后把这个包作为一个Response发送给客户端的代理类(Proxy Class)，同样地，这个代理类也对这个SOAP包进行解析处理，继而进行后续操作。

这就是WebService的一个运行过程。

### 下面对.net Remoting进行概括的阐述： 

.net Remoting 是在DCOM等基础上发展起来的一种技术，它的主要目的是实现跨平台、跨语言、穿透企业防火墙，这也是他的基本特点，与WebService有所不同的是，它支持HTTP以及TCP信道，而且它不仅能传输XML格式的SOAP包，也可以传输传统意义上的二进制流，这使得它变得效率更高也更加灵活。而且它不依赖于IIS，用户可以自己开发(Development)并部署(Dispose)自己喜欢的宿主服务器，所以从这些方面上来讲WebService其实上是.net Remoting的一种特例。

---

## web service和rpc的区别

### 1.rpc

RPC的全称叫远程过程调用，在过去一般传输的数据是二进制的，数据的传输形式相对轻量和简单，传输过程相对来说也要高效一些。直到后面XML-RPC的出现，RPC的传输形式相对来说要丰富一些，数据结构的传输也可以传输较为复杂的情况。

RPC不要求可以通过web的方式进行查看。

### 2.web service

web service的出现，可以说是在rpc发展的基础之上。web service是运行在web上的一个服务。他实现了 不同系统不同平台，不同开发语言和开发技术实现的软件系统之间的通讯。它的数据传输形式显得更加的丰富，除了以SOAP为代表的xml传输形式，还可以是JSON、protocol buffer等传输形式。web service可以通过web的方式进行查看。web service可以是B/S或者是C/S的架构。

### 3.应用

web service现在移动互联网的手机和平台以及平台和平台之间的通信使用极为广泛，RPC在一些后台的接口操作（推送、同步、命令操作）和系统间通信仍然在广泛的使用。
