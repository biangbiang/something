PHP mkdir()无写权限的问题解决方法
=================================

使用mkdir创建文件夹时，发现这个函数有两个参数，第二个参数是为新创建的文件夹指定权限。

但是如果直接用mkdir('文件地址', 0777);时 发现新文件夹的权限并不是777，一般情况下会是022。

因为mkdir在给文件夹制定权限时，会跟当前登录操作系统用户的umask（用户缺省权限属 性）值进行位“与”，得到的值才是最终权限值。

### umask 是什么？

我们创建文件的默认权限是怎么来的？如何改变这个默认权限呢？

当我们登录系统之后创建一个文件总是有一个默认权限的，那么这个权限是怎么来的呢？这就是 umask 干的事情。

umask 设置了用户创建文件的默认权限，它与 chmod 的效果刚好相反，umask 设置的是权限“补码”，而 chmod 设置的是文件权限码。一般在 /etc/profile、$HOME/.bash_profile 或 $HOME/.profile 中设置 umask 值。

### 如何计算 umask 值？

umask 命令允许你设定文件创建时的缺省模式，对应每一类用户(文件属主、同组用户、其他用户)存在一个相应的 umask 值中的数字。对于文件来说，这一数字的最大值分别是 6。系统不允许你在创建一个文本文件时就赋予它执行权限，必须在创建后用 chmod 命令增加这一权限。目录则允许设置执行权限，这样针对目录来说，umask 中各个数字最大可以到 7。

该命令的一般形式为：umask nnn，其中 nnn 可为 000 - 777。

我们只要记住 umask 是从权限中“拿走”相应的位即可。

如：umask 值为 022，则默认目录权限为 755，默认文件权限为 644。

所以，如果用户umask是022（一般默认是这个），即：000 010 010 在于mkdir指定的777 ，即：111 111 111 位“与”后，得到的真实权限为：022。

如果想让新建文件夹权限最大，有两种方法可以实现：（当然，是在当前用户能赋予最高权限的条件下）

1、修改用户umask，php提供有umask函数：

    $oldumask=umask(0); 
    mkdir('test',0777); 
    umask($oldumask);

这种方法看起来一劳永逸，在脚本开头文件里指定下umask值，后面直接用mkdir就可以控制权限，需要注意的是：在多线程服务器上使用umask函数时，多个线程会公用一个umask，所以可能会造成混乱。

2、使用chmod函数，这也是最常用的方法：

    mkdir('文件地址', 0777); 
    chmod('文件地址', 0777);

最后，需要注意一点，权限值最好使用八进制表示，即“0”开头，而且一定不要加引号。
