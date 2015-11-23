linux禁止用户远程登录的方法
===========================

linux禁止用户远程登录的方法：

1、在新添加一个新用户的时候就指定这个用户不能远程登录

    useradd  -s /sbin/nologin zgsj        //这里就是创建了一个不能远程登录的zgsj用户

    passwd zgsj         //修改zgsj用户密码

2、禁止个别用户登录。比如禁止zgsj用户登录。

    passwd -l zgsj    //锁定zsgj用户，禁止zgsj用户登录

这就话的意思是锁定zgsj用户，这样该用户就不能登录了。

    passwd -u zgsj    //解锁zsgj用户，允许zgsj用户登录

对锁定的用户zgsj进行解锁，用户可登录了。

3、我们通过修改/etc/passwd文件中用户登录的shell来实现linux禁止用户登录

    vi /etc/passwd

    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
    operator:x:11:0:operator:/root:/sbin/nologin
    games:x:12:100:games:/usr/games:/sbin/nologin
    zgsj:x:500:500::/home/lynn:/bin/bash

更改为：

    zgsj:x:500:500::/home/lynn:/sbin/nologin

该用户就无法登录了。

4、禁止所有用户登录。

    touch /etc/nologin

除root以外的用户不能登录了。 
