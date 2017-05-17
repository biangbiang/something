# 在https中传递查询字符串的安全性

我们常常听到这样一个问题：“参数是否可以在URLs中安全地传递到安全站点？“ 这个问题常常出现于客户看了HttpWatch的HTTPS请求后，想知道还有谁可以看到这些数据。

例如，假设在查询字符串参数中使用以下安全网址传递密码：

    https://www.httpwatch.com/?password=mypassword

HttpWatch能够显示安全请求的内容，因为它与浏览器集成，并且在用于HTTPS请求的SSL连接对数据进行加密之前查看数据

![](http://biangbiangpic.b0.upaiyun.com/blog/bba044fafb687d94b2195a393463a65d.jpg)

如果你看网络嗅探器，如Network Monitor，同一请求，你前后只会看到加密的数据。 在数据包跟踪中网址，标题或内容多是不可见的。

![](http://biangbiangpic.b0.upaiyun.com/blog/d8a06c5a768b0f8b3bd44cedd172a7a3.jpg)

您可以依赖一个安全的HTTPS请求，只要：

1、未忽略任何SSL证书警告

2、Web服务器用于启动SSL连接的私钥在Web服务器本身之外不可用。

因此，在网络层面，URL参数是安全的，但是其他一些途径会泄漏基于URL的数据：

1、URL存储在Web服务器日志中 - 特别是每个请求的整个URL都存储在服务器日志中。 这意味着URL中的任何敏感数据（例如密码）以明文形式保存在服务器上。 以下是使用查询字符串通过HTTPS发送密码时存储在httpwatch.com服务器日志中的条目：

    2009-02-20 10:18:27 W3SVC4326 WWW 208.101.31.210 GET /Default.htm password=mypassword 443 ...

存储明文密码通常不是一个好主意，即使是在服务器上。

2、网址存储在浏览器历史记录中 - 即使安全网页本身未缓存，浏览器也会将网址参数保存在其历史记录中。 以下是显示URL参数的IE历史记录

![](http://biangbiangpic.b0.upaiyun.com/blog/4fa474b45dfb76c7889b59535bea9d10.jpg)

如果用户创建书签，也会存储查询字符串参数。

3、URLs在Referrer头中传递 - 如果安全网页使用诸如javascript，图片或分析服务等资源，则该URL会在每个嵌入请求的 Referrer请求头中传递。 有时，查询字符串参数可以被传递到第三方站点并由其存储。 在HttpWatch中，您可以看到我们的密码查询字符串参数正在发送到Google Analytics：

![](http://biangbiangpic.b0.upaiyun.com/blog/b1d8888df0fa0c5630b1da6131e6ce13.jpg)

### 结论

解决这个问题需要两个步骤：

1、只在绝对必要的情况下传递敏感数据。 

2、如果用户被认证，最好使用具有有限生命周期的会话ID来标识它们。使用非持久会话级别的Cookie来保存会话ID和其他私人数据。

使用会话级Cookie来传递此信息的优点是：

它们不存储在浏览器历史记录中或磁盘上

它们通常不存储在服务器日志中

它们不会传递到嵌入式资源，例如图片或JavaScript库

它们仅适用于发出它们的域和路径

下面是在我们的在线商店中用于识别用户的ASP.NET会话cookie的示例：

![](http://biangbiangpic.b0.upaiyun.com/blog/e280c06f6a1f5aa43e763a3e1e18c93c.jpg)

请注意，该cookie仅限于域store.httpwatch.com，并且在浏览器会话结束时过期（即不会存储到磁盘）。

你当然可以在HTTPS中使用查询字符串参数，但在有可能暴露安全问题时不要使用它们。 例如，您可以安全地使用它们来标识部件号或显示的类型，但不要将它们用于密码，信用卡号码或其他不应公开的信息。 
