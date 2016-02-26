memcached常见命令
=================

1、启动Memcache 常用参数

    -p <num>      设置端口号(默认不设置为: 11211)

    -U <num>      UDP监听端口(默认: 11211, 0 时关闭) 

    -l <ip_addr>  绑定地址(默认:所有都允许,无论内外网或者本机更换IP，有安全隐患，若设置为127.0.0.1就只能本机访问)

    -d            独立进程运行

    -u <username> 绑定使用指定用于运行进程<username>

    -m <num>      允许最大内存用量，单位M (默认: 64 MB)

    -P <file>     将PID写入文件<file>，这样可以使得后边进行快速进程终止, 需要与-d 一起使用

如：

在linux下：

    ./usr/local/bin/memcached -d -u jb-mc -l 192.168.1.197 -m 2048 -p 12121

在window下：

    d:\App_Serv\memcached\memcached.exe -d RunService -l 127.0.0.1 -p 11211 -m 500

在windows下注册为服务后运行：

    sc.exe create jb-Memcached binpath= “d:\App_Serv\memcached\memcached.exe -d RunService -p 11211 -m 500″start= auto

    net start jb-Memcached
 

2、连接：

    telnet 127.0.0.1 11211

3、您将使用五种基本memcached 命令执行最简单的操作。这些命令和操作包括：

set: 用于向缓存添加新的键值对。如果键已经存在，则之前的值将被替换。

add :仅当缓存中不存在键时，add 命令才会向缓存中添加一个键值对。如果缓存中已经存在键，则之前的值将仍然保持相同，并且您将获得响应`NOT_STORED`。

replace:仅当键已经存在时，replace 命令才会替换缓存中的键。如果缓存中不存在键，那么您将从memcached 服务器接受到一条`NOT_STORED` 响应。

get:用于检索与之前添加的键值对相关的值。

delete:用于删除memcached 中的任何现有值。您将使用一个键调用delete ，如果该键存在于缓存中，则删除该值。如果不存在，则返回一条`NOT_FOUND` 消息。

gets:功能类似于基本的get 命令。两个命令之间的差异在于，gets 返回的信息稍微多一些：64 位的整型值非常像名称/值对的 “版本” 标识符。 

前三个命令是用于操作存储在memcached 中的键值对的标准修改命令。它们都非常简单易用，且都使用清单5 所示的语法：

    command <key> <flags> <expiration time> <bytes>

    <value>

    表1. memcached 修改命令参数

    参数       用法

    key  key 用于查找缓存值

    flags       可以包括键值对的整型参数，客户机使用它存储关于键值对的额外信息

    expiration time       在缓存中保存键值对的时间长度（以秒为单位，0 表示永远）

    bytes       在缓存中存储的字节点

    value       存储的值（始终位于第二行） 例子：

    set useID 0 0 5

    1234

4、缓存管理命令

最后两个memcached 命令用于监控和清理memcached 实例。它们是stats 和flush\_all 命令。

stats ：转储所连接的memcached 实例的当前统计数据。

flush\_all：用于清理缓存中的所有名称/值对。如果您需要将缓存重置到干净的状态，则flush\_all 能提供很大的用处。

查看memcached状态的基本命令，通过这个命令可以看到如下信息：

    STAT pid 22459                             进程ID

    STAT uptime 1027046                        服务器运行秒数

    STAT time 1273043062                       服务器当前unix时间戳

    STAT version 1.4.4                         服务器版本

    STAT pointer_size 64                       操作系统字大小(这台服务器是64位的)

    STAT rusage_user 0.040000                  进程累计用户时间

    STAT rusage_system 0.260000                进程累计系统时间

    STAT curr_connections 10                   当前打开连接数

    STAT total_connections 82                  曾打开的连接总数

    STAT connection_structures 13              服务器分配的连接结构数

    STAT cmd_get 54                            执行get命令总数

    STAT cmd_set 34                            执行set命令总数

    STAT cmd_flush 3                           指向flush_all命令总数

    STAT get_hits 9                            get命中次数

    STAT get_misses 45                         get未命中次数

    STAT delete_misses 5                       delete未命中次数

    STAT delete_hits 1                         delete命中次数

    STAT incr_misses 0                         incr未命中次数

    STAT incr_hits 0                           incr命中次数

    STAT decr_misses 0                         decr未命中次数

    STAT decr_hits 0                           decr命中次数

    STAT cas_misses 0    cas未命中次数

    STAT cas_hits 0                            cas命中次数

    STAT cas_badval 0                          使用擦拭次数

    STAT auth_cmds 0

    STAT auth_errors 0

    STAT bytes_read 15785                      读取字节总数

    STAT bytes_written 15222                   写入字节总数

    STAT limit_maxbytes 1048576                分配的内存数（字节）

    STAT accepting_conns 1                     目前接受的链接数

    STAT listen_disabled_num 0                

    STAT threads 4                             线程数

    STAT conn_yields 0

    STAT bytes 0                               存储item字节数

    STAT curr_items 0                          item个数

    STAT total_items 34                        item总数

    STAT evictions 0                           为获取空间删除item的总数
