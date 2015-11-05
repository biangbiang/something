使用/usr/bin/env的好处
=====================

在linux的一些脚本里，需在开头一行指定脚本的解释程序，如：

    #!/usr/bin/env python

再如：

    #!/usr/bin/env perl
    #!/usr/bin/env zimbu

但有时候也用

    #!/usr/bin/python

和

    #!/usr/bin/perl

那么 env到底有什么用？何时用这个呢？

脚本用env启动的原因，是因为脚本解释器在linux中可能被安装于不同的目录，env可以在系统的PATH目录中查找。同时，env还规定一些系统环境变量。

执行一下 env 命令后看看打印的内容

可以用env来执行程序：

    [mark@localhost ~]$ env python
    Python 2.6.6 (r266:84292, Dec  7 2011, 20:38:36)
    [GCC 4.4.6 20110731 (Red Hat 4.4.6-3)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 

而如果直接将解释器路径写死在脚本里，可能在某些系统就会存在找不到解释器的兼容性问题。有时候我们执行一些脚本时就碰到这种情况。
