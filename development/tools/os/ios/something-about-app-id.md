iOS开发——关于APP ID
===================

在开发iOS应用之前，需要在苹果开发者网站注册App ID 

App ID 组成为： 

    App ID Prefix（前缀）+'.'+ App ID Suffix（后缀） 

其中前缀是由苹果公司分配的，用来标识不同的开发者，也叫Team ID， 

后缀，也叫Bundle ID，是开发者自定义的标识，类似于Andriod开发中的包名， 

一般使用域名反转的风格： 

    com.test.myapp

值得注意的是，iOS开发中，App ID是可以被多个App公用的，这点和Android不太一样。 

例如我们可以设置Bundle ID为： 

    com.test.*

这样，就实现了App ID的公用。 

在注册App ID的时候，还可以选择一些服务(App Services)， 

比如信息推送（Push Notifications）、应用内支付（In-App Purchase） 

这样，就可以在App中使用苹果的这些服务了。
