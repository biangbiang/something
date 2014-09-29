MySQL UPDATE操作的一个注意事项
==============================

### 核心内容：

Mysql中update操作

对于没有真正更新数据的操作返回：(示例)

    0 rows affected
    rows matched: 1 changed: 0 warnings: 0


对于没有找到匹配记录的操作，返回：(示例)

    0 rows affected
    rows matched: 0 changed: 0 warnings: 0


### 情景描述：

项目中有这样的代码逻辑：

当有用户登入系统时，更新或添加用户的登录记录。

具体来说，如下：

    系统中有一张表，保存用户的登录信息（首次登录时间addtime，最后登录时间uptime，等）

    当登录接口被调用时，先尝试update用户的登录信息(uptime)，若update返回affected rows < 1则认为该用户是首次登入系统，进行insert操作。


这里有一个问题：

由于update操作只更新用户的uptime(最后登录时间)，uptime精确到秒。

	当用户在同一秒内登录两次时，第二次update操作将由于没有更改数据而返回
	
	affected_rows = 0，于是update操作被认为失败了，此时进行insert操作。
	
	如果数据库对user_id做了unique限定，则insert操作将失败，如果没做unique限定，则会有脏数据插入数据库。

这个问题有(至少)两个解决方案：

	a. 使用PHP的mysql_info()判断update操作返回的Rows match,但是这个函数返回值是字符串，需要自己取出其中的数字，略显麻烦；
	
	see http://www.php.net/manual/zh/function.mysql-info.php
	
	b. 先判断记录(user_id)是否存在，若存在则update，若不存在则insert；
	
	这个方案原理简单，但是多进行了一次数据库查询操作，有一定的代价。

后来项目选择了方案B，同时增加缓存层，具体来说如下：

    用户的登录信息来到后，先读从缓存中读取用户的历史信息，

        若缓存中没有历史信息，

            则去数据库中select，

                若select结果为空，

                    则insert；

                若select结果不为空，

                    则update，并将数据写入缓存；

        若缓存中有历史信息，

            则直接进行update操作

这样做看似解决了问题，但是还是有可能发生insert错误：

    那就是当同一个用户并发过来两次登录。
   
    如果这个用户是老用户，以前登过系统，则缓存或者数据库中肯定有
   
    它的数据，只会做update操作，这不会有问题。
   
    但是如果这个用户是第一次登入项目，则有可能导致如下情形：
   
    两个请求都去读缓存，都没读到；然后都去读db，又都没读到；
   
    于是都去insert，问题就出来了，只能有一个insert成功，另一个应该
   
    失败，或者插入脏数据。

所以这样解决并不合理，我想这样处理也许可以解决这个问题：

    当登录信息到来时，先尝试insert操作，当insert失败时(数据库中uniqe约束不允许重复user_id)，再进行update操作。


我忽然发现：insert失败好像并不是什么“问题”啊，就像我最后提出的方案，不也是用insert失败来判断下一步的行为吗？


### 后记

文中后半部分提出的问题其实是一个并发的问题，留待下回分解。

另外，Mysql的INSERT的ON DUPLICATE KEY UPDATE功能(若insert存在冲突键则update)，也可以用来解决文中前半部分提到的问题。

see http://dev.mysql.com/doc/refman/5.1/zh/sql-syntax.html#insert


### 相关知识：

MySQL INSERT ... SELECT语法 (用于快速从一个或多个表或多个表中插入数据)

see http://dev.mysql.com/doc/refman/5.1/zh/sql-syntax.html#insert-select
