easy_install和pip
=================

作为Python爱好者，如果不知道easy\_install或者pip中的任何一个的话，那么......
 
easy\_insall的作用和perl中的cpan，ruby中的gem类似，都提供了在线一键安装模块的傻瓜方便方式。

而pip是easy\_install的改进版，提供更好的提示信息，删除package等功能。

老版本的python中只有easy\_install，没有pip。

easy\_install的用法：

1） 安装一个包

    $ easy_install <package_name>
    $ easy_install "<package_name>==<version>"

2) 升级一个包

    $ easy_install -U "<package_name>>=<version>"

pip的用法

1) 安装一个包

    $ pip install <package_name>
    $ pip install <package_name>==<version>

2) 升级一个包 (如果不提供version号，升级到最新版本）

    $ pip install --upgrade <package_name>>=<version>

3）删除一个包

    $ pip uninstall <package_name> 
