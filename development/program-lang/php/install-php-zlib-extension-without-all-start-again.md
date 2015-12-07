不重新编译PHP为PHP安装zlib扩展
==============================

一台服务器，编译PHP时未设置参数，导致缺少zlib扩展，无法执行解压缩，错误信息是：“Fatal error: Call to undefined function gzopen”。

首先找到当初编译PHP时的目录，如果没了就找一个相同版本的解压缩，解压缩到php（假设）目录下，然后以root身份进入php/ext/zlib/目录下

执行phpize，报错：“Cannot find config.m4.”，我找了半天没找到这个文件，但是看到一个类似的“config0.m4”，就cp了一个：

    cp config0.m4 config.m4

这个解决方法有点山寨，我也不知道啥意思，为什么多了一个0，反正亲自测了能行。

再执行phpize，又报错：“Cannot find autoconf. Please check your autoconf installation and the $PHP_AUTOCONF environment variable. Then, rerun this script.”

原来是autoconf不存在，通过yum安装：

    yum -y install autoconf

再执行phpize，嗯，再敢报错看我不打断你的狗腿！！这次顺利的执行完了。

    # phpize
    Configuring for:
    PHP Api Version:         20100412
    Zend Module Api No:      20100525
    Zend Extension Api No:   220100525

在这个目录就生成了一个configure脚本，再执行以下代码获得php-config脚本的位置备用：

    # which php-config
    /usr/local/bin/php-config（你的电脑上未必是这个结果）

再执行以下代码获得zlib的位置备用：

    # find / -name zlib.h
    /usr/include/zlib.h

都准备好之后就开始执行配置

    ./configure --with-php-config=/usr/local/bin/php-config --with-zlib=/usr

注意：--with-zlib=/usr这里不需要把文件目录写全，写到这里就行了，PHP的扩展在编译时都是这个德性，习惯就好了。

然后就是正式的编译和安装了：

    make && make install

顺利的话，最终会提示：

    Installing shared extensions:     /usr/local/lib/php/extensions/no-debug-non-zts-20100525/

然后在php.ini的最后增加这么一句：

    extension=zlib.so

再重启php的CGI或者FastCGI或者php-fpm就可以了，具体重启什么要看服务器通过什么方式运行的PHP。

一切顺利的话，在phpinfo中就可以看到zlib的信息了，这就证明PHP的zlib扩展已经顺利的安装成功了。
