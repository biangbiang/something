Ubuntu的bash和dash的区别
========================

### 什么是bash ？

Bash(GNU Bourne-Again Shell)是许多Linux平台的内定Shell，事实上，还有许多传统UNIX上用的Shell，像tcsh、csh、ash、bsh、ksh等等，Shell Script大致都类同，当您学会一种Shell以后，其它的Shell会很快就上手，大多数的时候，一个Shell Script通常可以在很多种Shell上使用

### 什么是dash ？

dash is the standard command interpreter for the system.  The current

version of dash is in the process of being changed to conform with the

POSIX 1003.2 and 1003.2a specifications for the shell.

先用命令ls -l /bin/sh 看看

![](http://biangbiangpic.b0.upaiyun.com/blog/689c0d41501001ecd317bccd22348c3a.png)

我们会发现Ubuntu默认采用的是 dash

如果要修改默认的sh，可以采用命令

    sudo dpkg-reconfigure dash

然后选择【否】

![](http://biangbiangpic.b0.upaiyun.com/blog/785c2770d923ed57716fc696fe3a8e42.png)

成功后再执行ls -l /bin/sh 看看

![](http://biangbiangpic.b0.upaiyun.com/blog/f44a8972df4e2c4fc8bf3f96e54e1f2b.png)
