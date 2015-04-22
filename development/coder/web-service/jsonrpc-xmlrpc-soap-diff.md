JSON-RPC、XML-RPC、SOAP三者的关系
=================================

JSON-RPC规范：[http://json-rpc.org/wiki/specification](http://json-rpc.org/wiki/specification)

XML-RPC规范：[http://www.xmlrpc.com/spec](http://www.xmlrpc.com/spec)

SOAP规范：[http://www.w3.org/TR/2000/NOTE-SOAP-20000508/#\_Toc478383487](http://www.w3.org/TR/2000/NOTE-SOAP-20000508/#_Toc478383487)

三者都是为了实现RPC中的消息交换，并且都没有定义传输协议。不过为了更方便在网络中传输，而且由于HTTP的无状态性，都使得HTTP为这三者的常用的传输协议。下面例子也是基于HTTP协议的

XML-RPC和SOAP都是基于XML格式的消息交换：

XML-RPC非常简单，定义了几种基本类型、匿名结构体、匿名数组；

SOAP除了基本类型、命名结构体、命名数组以外，还可以自定义类型，能使用多态的方法调用方式

而JSON-RPC是基于JSON格式的消息交换，JSON比XML更加轻巧，并且非常容易在页面JS中使用，其他特点与XML-RPC类似

下面是使用这几种协议发送请求的例子：

**XML-RPC**

    POST /RPC2 HTTP/1.0  
    User-Agent: Frontier/5.1.2 (WinNT)  
    Host: betty.userland.com  
    Content-Type: text/xml  
    Content-length: 181  
      
      
      
    <?xml version="1.0"?>  
    <methodCall>  
       <methodName>examples.getStateName</methodName>  
       <params>  
          <param>  
             <value><i4>41</i4></value>  
             </param>  
          </params>  
       </methodCall>  

**SOAP**

    POST /StockQuote HTTP/1.1  
    Host: www.stockquoteserver.com  
    Content-Type: text/xml; charset="utf-8"  
    Content-Length: nnnn  
    SOAPAction: "Some-URI"  
      
    <SOAP-ENV:Envelope  
      xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"  
      SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/>  
       <SOAP-ENV:Header>  
           <t:Transaction  
               xmlns:t="some-URI"  
               SOAP-ENV:mustUnderstand="1">  
                   5  
           </t:Transaction>  
       </SOAP-ENV:Header>  
       <SOAP-ENV:Body>  
           <m:GetLastTradePrice xmlns:m="Some-URI">  
               <symbol>DEF</symbol>  
           </m:GetLastTradePrice>  
       </SOAP-ENV:Body>  
    </SOAP-ENV:Envelope>  

**JSON**

    --> { "method": "echo", "params": ["Hello JSON-RPC"], "id": 1}  
    <-- { "result": "Hello JSON-RPC", "error": null, "id": 1}  
