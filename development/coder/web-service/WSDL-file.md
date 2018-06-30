WebService 之 WSDL文件 讲解
===========================

恩，我想说的是，是不是经常有人在开发的时候，特别是和第三方有接口的时候，走的是SOAP协议，然后用户给你一个WSDL文件，说按照上面的进行适配，嘿嘿，这个时候，要是你以前没有开发过，肯定会傻眼，那如果你想学习的话，就认真的看下面的讲解咯：

### 一、WSDL概述

WebServices Description Language (WSDL Web服务语言)是一个用于精确描述Web Service的文档格式。

WSDL非常适合于用作代码生成器，它能够读取WSDL文档，并且可以为访问Web服务生成一个程序化的接口，大多数软件供应商和主要的标准机构（包括 W3C、WS-I和OASIS）都支持WSDL。例如：JAX-RPC provider（例如：BEA Weblogic）通过API用WSDL生成相应的占位程序；IBM WebSphere、Microsoft.NET以及Apache Axis都有自己的工具生成相关的代码。下图是一个例子：

![](http://biang.io/biangpic/blog/7b2f7821f64680f7d45e351e4edc69e7.jpg)

上面的例子JAX-RPC通过读取WSDL文档，创建JAX-RPC RMI接口（endpoint接口）和实现此接口的网络占位程序（stub）。客户端程序通过RMI接口，Stub和Web Service服务端交换SAOP消息。

### 二、WSDL基本结构

WSDL文档是一个遵循WSDL XML模式的XML文档（文档实例）；类似于：SOAP文档是一个遵循SOAP XML模式的XML文档（文档实例）；

一个WSDL文档的根元素是definitions元素，WSDL文档包含7个重要的元素：types, import, message, portType, operations, binding和service元素。

### 三、WSDL声明
      
#### 3.1 XML声明

    <?xml version="1.0" encoding="UTF-8"?>

WSDL的声明必须定义成使用：UTF-8 或者UTF-16 编码。

#### 3.2 definition元素

所有WSDL文档的根元素都是definition元素。   

    <definitions name="BookQuoteWS"
        targetNamespace="http://www.Monson-Haefel.com/jwsbook/BookQuote"
        xmlns:mh="http://www.Monson-Haefel.com/jwsbook/BookQuote"
        xmlns:soapbind="http://schemas.xmlsoap.org/wsdl/soap/"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        xmlns="http://schemas.xmlsoap.org/wsdl/">

definition元素中一般包括若干个XML命名空间；

http://schemas.xmlsoap.org/wsdl/是默认的命名空间，这样就可以不用显式地定义每一个WSDL元素的命名空间了，例如：<types> <messages> <portType>…；文档中所有的元素缺省应该属于这个命名空间。

definition元素的的一个属性是name，此属性不重要可以没有；

定义了targetNamespace命名空间，它为在模式中显式创建的所有新类型均声明了XML命名空间，而且上面的例子中赋予了mh前缀。

    <!-- message elements describe the input and output parameters -->

    <message name="GetBookPriceRequest">

          <part name="isbn" type="xsd:string" />

    </message>

    <message name="GetBookPriceResponse">

         <part name="price" type="xsd:float" />

    </message>

    <!-- portType element describes the abstract interface of a Web service -->

    <portType name="BookQuote">

        <operation name="getBookPrice">

              <input name="isbn" message="mh:GetBookPriceRequest"/>

              <output name="price" message="mh:GetBookPriceResponse"/>

      </operation>

    </portType>
 

上面的例子中：message元素利用name属性指定了标签（例如：GetBookPriceRequest），这些标签会自动使用targetNamespace的命名空间，标签了的messages元素通常被称为定义。

文档中的其他元素用标签和命名空间前缀去应用定义，例如上面的例子中：input元素是使用mh:GetBookPriceRequest来引用标签GetBookPriceRequest。

#### 3.3 Types元素

Types元素用作一个容器，定义了自定义的特殊数据类型，在声明消息部分（有效负载）的时候，messages定义使用了types元素中定义的数据类型与元素。

    <?xml version="1.0" encoding="UTF-8"?>

    <definitions name="BookQuoteWS"
                          targetNamespace="http://www.Monson-Haefel.com/jwsbook/BookQuote"
                          xmlns:mh="http://www.Monson-Haefel.com/jwsbook/BookQuote"
                          xmlns:soapbind="http://schemas.xmlsoap.org/wsdl/soap/"
                          xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                         xmlns="http://schemas.xmlsoap.org/wsdl/">
    <types>

        <xsd:schema   targetNamespace="http://www.Monson-Haefel.com/jwsbook/BookQuote">

          <!-- The ISBN simple type -->

          <xsd:simpleType name="ISBN">

            <xsd:restriction base="xsd:string">

              <xsd:pattern value="[0-9]{9}[0-9Xx]" />

            </xsd:restriction>

          </xsd:simpleType>

        </xsd:schema>

    </types>

Types元素作为一个容器，用来定义XML模式内置的数据类型（即复杂类型和定制的简单类现，详细见Web Service XML文章）中没有描述的各种数据类型。例如：ISBN。

上面的例子中，types元素中直接嵌套了一个完整的W3C XML模式文档，此文档中targetNamespace必须是一个有效的非空值，而且必须属于由WSDL文档。

#### 3.4 Import元素

Import元素可以让当前的文档使用其他WSDL文档中指定命名空间中的定义。 

    <definitions name="AllMhWebServices"
             xmlns="http://schemas.xmlsoap.org/wsdl/">

        <import namespace="http://www.Monson-Haefel.com/jwsbook/BookQuote"

         location="http://www.Monson-Haefel.com/jwsbook/BookPrice.wsdl"/>

        <import namespace="http://www.Monson-Haefel.com/jwsbook/po"

         location="http://www.Monson-Haefel.com/jwsbook/wsdl/PurchaseOrder.wsdl"/>

        <import namespace="http://www.Monson-Haefel.com/jwsbook/Shipping"

         location="http://www.Monson-Haefel.com/jwsbook/wsdl/Shipping.wsdl"/>

    </definitions >

WSDL的import元素必须声明两个属性，即namespace属性和location属性。

namespace属性必须和正导入的WSDL文档中声明的targetNamespace相匹配。

location属性必须指向一个实际的WSDL文档。

### 四、WSDL抽象接口

Message、portType和operation元素用于描述Web服务的抽象接口，相当于JAVA或者C++中编程中的类的接口。其中portType相当于类接口的名称；operation相当于接口中包含的函数，message相当于函数的参数和返回值。
        
#### 4.1 Message元素

Message元素描述了Web服务的有效负载。相当于函数调用中的参数和返回值。

    <message name="GetBulkBookPriceRequest">

        <part name="isbn" type="xsd:string"/>

        <part name="quantity" type="xsd:int"/>

      </message>

      <message name="GetBulkBookPriceResponse">

        <part name="price" type="mh:prices" />

      </message>

RPC式样的Web服务的message服务

GetBulkBookPriceRequest表示消息的输入（相当于函数的参数），GetBulkBookPriceResponse表示消息的输出（相当于函数的返回值）

Web Service的输入和输出参数可以是多个（一个特点），每一个输入或者输出使用part元素定义，RPC样式必须使用type来定义类型

RPC样式用类型来数据定义过程调用，调用中的每一个元素表示某一个类型的参数。

    <types>

        <xsd:schema targetNamespace="http://www.Monson-Haefel.com/jwsbook/PO">

          <!-- Import the PurchaseOrder XML schema document -->

          <xsd:import namespace="http://www.Monson-Haefel.com/jwsbook/PO"

           schemaLocation="http://www.Monson-Haefel.com/jwsbook/po.xsd" />

        </xsd:schema>

      </types>

      <!-- message elements describe the input and output parameters -->

      <message name="SubmitPurchaseOrderMessage">

        <part name="order" element="mh:purchaseOrder" />

      </message>

文档式样Web服务的Messages元素：

当用户采用文档式样消息传递模式的时候，messages元素要应用types定义中的顶级元素。具体顶级元素的定义和XML schema详见Web Server XML文档。

消息部分使用element属性定义

文档式样的消息传递要交换XML文档，并且应用它们的顶级元素。

注：Messages元素的RPC/Document试样对应了SOAP RPC/Document消息传递模式，详细见Web Server SOAP相关文档

    <types>

        <xsd:schema targetNamespace="http://www.Monson-Haefel.com/jwsbook/PO">

          <!-- Import the PurchaseOrder XML schema document -->

          <xsd:element name="InvalidIsbnFaultDetail" >

            <xsd:complexType>

              <xsd:sequence>

                <xsd:element name="offending-value" type="xsd:string"/>

                <xsd:element name="conformance-rules" type="xsd:string" />

              </xsd:sequence>

            </xsd:complexType>

          </xsd:element>

        </xsd:schema>

      </types>

     

      <!-- message elements describe the input and output parameters -->

      <message name="GetBookPriceRequest">

        <part name="isbn" type="xsd:string" />

      </message>

      <message name="GetBookPriceResponse">

        <part name="price" type="xsd:float" />

      </message>

      <message name="InvalidArgumentFault">

        <part name="error_message" element="mh:InvalidIsbnFaultDetail" />

      </message>

 

声明错误消息：

错误使用的消息定义只能采用Document/Literal编码样式

上面声明了匿名类型，InvalidIsbnFaultDetail不需要type类型，complexType中也不包括name属性，详细见Web Service XML相关文档。

#### 4.2 portType元素

PortType元素定义了Web服务的抽象接口，它可以由一个或者多个operation元素，每个operation元素定义了一个RPC样式或者文档样式的Web服务方法。

#### 4.3 operation元素

Operation元素要用一个或者多个messages消息来定义它的输入、输出以及错误。

    <message name="GetBulkBookPriceRequest">

      <part name="isbn" type="xsd:string"/>

      <part name="quantity" type="xsd:int"/>

    </message>

    <message name="GetBulkBookPriceResponse">

      <part name="prices" type="mh:prices" />

    </message>

    <message name="InvalidArgumentFault">

        <part name="error_message" element="mh:InvalidIsbnFaultDetail" />

      </message>

    <portType name="GetBulkBookPrice" >

      <operation name="getBulkBookPrice" parameterOrder="isbn quantity">

         <input name="request" message="mh:GetBulkBookPriceRequest"/>

         <output name="prices" message="mh:GetBulkBookPriceResponse"/>

    <fault name="InvalidArgumentFault" message="mh:InvalidArgumentFault"/>

      </operation>

    </portType>

Input表示传递到Web服务的有效负载；output表示返回给客户的有效负载；可以不包括，也可以包括一个或者多个fault错误消息。

parameterOrder定义了input和output消息采用的正确的顺序

使用parameterOrder的时候，必须包含所有输入参数部分；并且只包含不是返回类型的输出部分，如果output只有一个part（上例），会假设返回值，所以不包括在parameterOrder中

如果parameterOrder列出output中的part部分，那么这个将被作为OUT参数，如果input元素和output元素使用相同的名称声明了一个部分的时候，此部分为INOUT参数

#### 4.4 WSDL消息交换模式（MEP）

Messaging Exchange Patterns(MEP)

Web服务中使用了四种消息交换模式，即请求/响应、单向、通知以及恳求/响应模式。大多数基于WSDL的web服务使用请求/响应和单向两种模式。

WSDL通过operation元素的input/output来定义使用那种模式，如果有input+output+可选的fault参数，那就使用请求/响应模式；如果只使用input，那就使用单向模式。

在通知模式中：Web服务将消息发送给客户，但不等待回复；一般客户通过注册来接收通知；在恳求/响应模式中类似通知模式，唯一的区别要期待客户对Web服务的响应。

### 五、WSDL实现：binding元素

Binding元素将一个抽象的portType映射到一组具体的协议（SOAP或者HTTP）、消息传递样式（RPC或者document）以及编码样式（literal或者SOAP encoding）。

Binding的类似于将接口或者函数的调用绑定到某种协议上：例如CORBA、COM或者RPC的方式，这里使用SOAP协议。

#### 5.1 soapbind:binding元素

    <binding name="BookPrice_Binding" type="mh:BookQuote">

      <soapbind:binding style="rpc"

       transport="http://schemas.xmlsoap.org/soap/http"/>

      <operation name="getBookPrice">

soapbind:binding元素指定了用于传输SOAP消息的Internet协议以及operation缺省的消息类型（RPC还是文档类型）

http://schemas.xmlsoap.org/soap/http表示采用的是HTTP的传输方式，当然也可以用HTTPS,用户具体使用HTTP还是HTTPS取决于Port元素中定义的location属性声明中的模式。

上面的rpc表示缺省状态下：operation将采用RPC的方式传递消息负载。

#### 5.2 soapbind:operation元素

    <operation name="getBookPrice">

        <soapbind:operation style="rpc"

         soapAction=

         "http://www.Monson-Haefel.com/jwsbook/BookQuote/GetBookPrice"/>

    POST 1ed/BookQuote HTTP/1.1

    Host: www.Monson-Haefel.com

    Content-Type: text/xml; charset="utf-8"

    Content-Length: nnnn

    SOAPAction="http://www.Monson-Haefel.com/jwsbook/BookQuote/GetBookPrice"

soapbind:operation元素指定了消息传递样式（RPC或者document），并且指定了SOAPAction字段的值。

上面的例子显示在HTTP消息中的SOAPAction中对应的值

#### 5.3 soapbind:body元素

    <operation name="getBookPrice">

    <soapbind:operation style="rpc"/>

    <input>

              <soapbind:body use="literal"

              namespace="http://www.Monson-Haefel.com/jwsbook/BookQuote" />

           </input>

           <output>

              <soapbind:body use="literal"

              namespace="http://www.Monson-Haefel.com/jwsbook/BookQuote" />

           </output>

    <operation name="submit">

      <soapbind:operation style="document"/>

          <input>

            <soapbind:body use="literal" />

          </input>

          <output>

            <soapbind:body use="literal" />

          </output>

    </operation>

soapbind:body元素有四个属性use、namespace、part和encodingStyle

对于WS-I use的属性值必须是literal，意味这不是用编码的方式，所以永远不会用到encodingStyle属性

在RPC样式中，必须用一个有效的URI指定的namespace属性。此URI可以于WSDL文档的targetNampspce相同；而在document样式中不能使用namespace，XML文档样式的命名空间派生于它的XML文档

#### 5.4 soapbind:fault元素

    <fault name="InvalidArgumentFault">

         <soapbind:fault name="InvalidArgumentFault" use="literal" />

    </fault>

    <portType name="BookQuote">

      <operation name="getBookPrice">

         <input name="isbn" message="mh:GetBookPriceRequest"/>

         <output name="price" message="mh:GetBookPriceResponse"/>

         <fault name="InvalidArgumentFault" message="mh:InvalidArgumentFault"/>

      </operation>

    </portType>

soapbind:fault元素和fault元素包含一个强制性的name属性，表示要引用声明于对应portType中的专有错误消息

#### 5.5 soapbind:header元素

    <types>

    <xsd:schema 

    targetNamespace="http://www.Monson-Haefel.com/jwsbook/BookQuote"

         xmlns="http://www.w3.org/2001/XMLSchema">

            <xsd:element name="message-id" type="string" />

        </xsd:schema>

    </types>

     

    <!-- message elements describe the input and output parameters -->

      <message name="Headers">

        <part name="message-id" element="mh:message-id" />

      </message>

     

    <operation name="getBookPrice">

      <input>

         <soapbind:header message="mh:Headers" part="message-id" use="literal" />

         <soapbind:body use="literal"

               namespace="http://www.Monson-Haefel.com/jwsbook/BookQuote" />

            </input>

WSDL在绑定的input元素、output元素中利用soapbind:header元素显式指定了一个SOAP头文件

#### 5.6 soapbind:headerfault元素  

    <!-- message elements describe the input and output parameters -->

      <message name="HeaderFault">

        <part name="faultDetail" element="mh:detailMessage" />

      </message>

     

    <input>

          <soapbind:header message="mh:Header" use="literal">

             <soapbind:headerfault message="mh:Headers" use="literal" />

          </soapbind:header>

          <soapbind:body use="literal"

               namespace="http://www.Monson-Haefel.com/jwsbook/BookQuote" />

       </input>

soapbind:headerfault元素表述了Header专用的错误消息，如果有一个响应消息，必须在消息的Header元素中返回各种header的专用错误。

SOAP没有就如何提供Header错误方面给出详细说明，只是要求必须在Header元素中包含detail元素。有些SOAP工具箱将SOAP的fault放在header元素中。

### 六、WSDL实现：Service和Port元素

    <service name="BookPriceService">

      <port name="BookPrice_Port" binding="mh:BookPrice_Binding">

        <soapbind:address location=

         "http://www.Monson-Haefel.com/jwsbook/BookQuote" />

      </port>

      <port name="BookPrice_Failover_Port" binding="mh:BookPrice_Binding">

        <soapbind:address location=

         "http://www.monson-haefel.org/jwsbook/BookPrice" />

      </port>

      <port name="SubmitPurchaseOrder_Port"

       binding="mh:SubmitPurchaseOrder_Binding">

        <soapbind:address location=

         "https://www.monson-haefel.org/jwsbook/po" />

      </port>

    </service>

Service元素包含一个或者多个Port元素

每一个Port元素对应一个不同的Web服务，port将一个URL赋予一个特定的binding，通过location实现

可以使两个或者多个port元素将不同的URL赋给相同的binding，例如负载平衡和容错的时候，使用这种方法。

soapbind:address：将Internet地址通过location属性赋予一个SOAP绑定。
