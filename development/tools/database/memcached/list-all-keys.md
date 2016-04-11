Memcache 查看列出所有key方法
============================

今天在做一个Memcache的session测试，但是在测试的过程中，发现Memcache没有一个比较简单的方法可以直接象redis那样keys *列出所有的Session key，并根据key get对应的session内容，于是，我开始查找资料，翻出来的大部分是一些memcache常用命令等，但是对列出key的办法，讲解却不多，于是来到google，找到了一个国外的资料
 
具体的内容我套用我的测试环境中，操作如下

### 1. cmd上登录memcache

    > telnet 127.0.0.1 11211

### 2. 列出所有keys

    stats items // 这条是命令
    STAT items:7:number 1 
    STAT items:7:age 188
    END

### 3. 通过itemid获取key

接下来基于列出的items id，本例中为7，第2个参数为列出的长度，0为全部列出

    stats cachedump 7 0 // 这条是命令
    ITEM Sess_sidsvpc1473t1np08qnkvhf6j2 [183 b; 1394527347 s]
    END

### 4. 通过get获取key值

上面的stats cachedump命令列出了我的session key，接下来就用get命令查找对应的session值

    get Sess_sidsvpc1473t1np08qnkvhf6j2 //这条是命令
    VALUE
    Sess_sidsvpc1473t1np08qnkvhf6j2 1440 1
     83
     Sess_|a:5:{s:6:"verify";s:32:"e70981fd305170c41a5632b2a24bbcaa";s:3:"uid";s:1:"1
     ";s:8:"username";s:5:"admin";s:9:"logintime";s:19:"2014-03-11 16:24:25";s:7:"log
     inip";s:9:"127.0.0.1";}

### 5. 参考地址

参考地址：http://www.darkcoding.net/software/memcached-list-all-keys/
