chrome笔记
==========

### chrome如何清除某一个网站的cookie？

	chrome://settings/cookies

### Failed to load resource: net::ERR\_CACHE\_MISS 这个是什么问题啊，大神求救

这个问题我也碰到了，找了下原因，可以这样理解，Failed to load resource: net::ERR\_CACHE\_MISS 开发人员工具载入缓存的时候，说找不到资源。 

问题根本在于你先打开页面，再打开chrome的开发人员工具。而页面本身设置了no-store 无缓存，所以后者打开的开发人员工具去不到缓存。 

如果你已经打开开发者工具的时候，再刷新就不会有这个错误了，同时可以看下页面的头信息： 

    HTTP/1.1 200 OK 
    Content-Type: text/html; charset=utf-8 
    Transfer-Encoding: chunked 
    Connection: keep-alive 
    Expires: Thu, 19 Nov 1981 08:52:00 GMT 
    Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0 
    Pragma: no-cache 
    Content-Encoding: gzip
