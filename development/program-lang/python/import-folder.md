Python引用（import）文件夹下的py文件的方法
==========================================

这篇文章主要介绍了Python引用（import）文件夹下的py文件的方法,Python中比较特别,导入文件夹下的py文件,则这个目录下必须要有一个__init__.py文件才可,需要的朋友可以参考下

Python的import包含文件功能就跟PHP的include类似，但更确切的说应该更像是PHP中的require，因为Python里的import只要目标不存在就报错程序无法往下执行。要包含目录里的文件，PHP中只需要给对路径就OK。Python中则不同，下面来看看这个例子。

目录结构：

![](http://biangbiangpic.b0.upaiyun.com/blog/5e509f045dc62ff5a946ba6a83f40216.jpg)

a.py 要 import dir目录下的 b.py 文件。a.py代码如下：


    # coding=utf-8
    "import dir 目录下的 b.py 文件"
     
    import dir.b
     
    print dir.b.name

执行 a.py 报错

![](http://biangbiangpic.b0.upaiyun.com/blog/9b3c9de4edc837b83f7878509c32fc0a.jpg)

提示找不到这个模块的名字 dir.b 。通过查找官方文档，发现要包含目录下的文件时需要在目录下声明一个__init__.py文件，即使这个文件是空的也可以。当然这个文件也可以初始一些数据。

于是在 dir 下新建 __init__.py文件，目录结构如下：

![](http://biangbiangpic.b0.upaiyun.com/blog/8e85ee02c168b818d4db19d7c9869704.jpg)

重新执行a.py，一切OK！
