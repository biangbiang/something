Python:urllib 和urllib2之间的区别
=================================

作为一个Python菜鸟，之前一直懵懂于urllib和urllib2，以为2是1的升级版。今天看到老外写的一篇《Python: difference between urllib and urllib2》才明白其中的区别。

 

You might be intrigued by the existence of two separate URL modules in Python -urllib and urllib2. Even more intriguing: they are not alternatives for each other. So what is the difference between urllib and urllib2, and do we need them both? 

你可能对于Python中两个独立存在的-urllib2和-urllib2感到好奇。更有趣的是：它们并不是可以相互代替的。那么这两个模块间的区别是什么，并且这两个我们都需要吗？

urllib and  urllib2are both Python modules that do URL request related stuff but offer different functionalities. Their two most significant differences are listed below: 

urllib 和urllib2都是接受URL请求的相关模块，但是提供了不同的功能。两个最显著的不同如下：

urllib2 can accept a Request object to set the headers for a URL request,urllib accepts only a URL. That means, you cannot masquerade your User Agent string etc.

urllib2可以接受一个Request类的实例来设置URL请求的headers，urllib仅可以接受URL。这意味着，你不可以伪装你的User Agent字符串等。
 
urllib provides the urlencode method which is used for the generation of GET query strings, urllib2 doesn't have such a function. This is one of the reasons why urllib is often used along with urllib2.

urllib提供urlencode方法用来GET查询字符串的产生，而urllib2没有。这是为何urllib常和urllib2一起使用的原因。

For other differences between urllib and urllib2 refer to their documentations, the links are given in the References section. 

Tip: if you are planning to do HTTP stuff only, check out httplib2, it is much better than httplib or urllib or urllib2. 

提示：如果你仅做HTTP相关的，看一下httplib2，比其他几个模块好用。

---

Python: difference between urllib and urllib2
=============================================

**What is the difference between urllib and urllib2 modules of Python?**

You might be intrigued by the existence of two separate URL modules in Python - urllib and urllib2. Even more intriguing: they are not alternatives for each other. So what is the difference between urllib and urllib2, and do we need them both?

urllib and urllib2 are both Python modules that do URL request related stuff but offer different functionalities. Their two most significant differences are listed below:

* urllib2 can accept a Request object to set the headers for a URL request, urllib accepts only a URL. That means, you cannot masquerade your User Agent string etc.

* urllib provides the urlencode method which is used for the generation of GET query strings, urllib2 doesn't have such a function. This is one of the reasons why urllib is often used along with urllib2.

For other differences between urllib and urllib2 refer to their documentations, the links are given in the References section.

Tip: if you are planning to do HTTP stuff only, check out httplib2, it is much better than httplib or urllib or urllib2.

### Exercise

What is User Agent?

What is the difference between GET and POST request methods?

What is HTTP?

### References

[urllib](https://docs.python.org/2/library/urllib.html)

[urllib2](https://docs.python.org/2/library/urllib2.html)
