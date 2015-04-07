install php5.2.17 with php-fpm
==============================

### 引言

这段时间打算在站点上添加新闻功能，但是鉴于之前的经验，觉得这个事情自己开发实在时太不值当了（需求、原型、效果图、网页重构、前后端代码、管理后台代码……还不包括维护和升级的成本），所以决定搭建一个discuz!站点，用这个来做实际的新闻录入和展示，然后在主站上做一个新闻推荐系统就行了。

之前两年一直在搞python，php说实话没看过两眼。好吧，今天就来搞一搞php。

### 正文

经以前同事文总建议，我装了php5.2.17（http://www.php.net/get/php-5.2.17.tar.gz/from/a/mirror）

不用参数的话，安装其实是很快的（./configure && make && make install）。

可蛋疼的是，据说php自带的那个cgi不给力，所以需要用一个叫php-fpm的玩意来patch一下，使用fastcgi来跑，于是开始安装php-fpm。（5.3以上版本已经自带了，详细参见http://php-fpm.org/download/）

在 http://php-fpm.org/downloads/ 上找到对应的php-fpm版本，然后下载之。下载下来的是一个补丁文件，所以需要如下方式安装：

    tar zxvf php-5.2.17.tar.gz
    gzip -cd php-5.2.17-fpm-0.5.14.diff.gz | patch -d php-5.2.17 -p1

这样补丁就打好了。

然后安装步骤也稍稍改变一下……在configure时多加了一些参数。（请注意mysql参数，改为你自己的mysql对应路径。）

    ./configure  --prefix=/usr/local/php-5.2.17 --with-config-file-path=/usr/local/php-5.2.17/etc --with-mcrypt --with-gettext --with-gd --with-jpeg-dir --with-png-dir --with-ttf --with-curl --with-freetype-dir --enable-gd-native-ttf --enable-mbstring --enable-sockets --with-png-dir --with-pdo-mysql --with-zlib  --enable-fastcgi --enable-fpm --enable-force-cgi-redirect --with-mysql=/usr/local/mysql --with-mysqli=/usr/local/mysql/bin/mysql_config
    make
    make install


### 问题小结

#### A.

在configure中途碰到了一些奇怪的问题，说是找不到libjpeg和libpng。这两个我都装了，应该是执行configure的时候没有到对应的路径中去查找这些文件。

百度了一下，得到解决方案：

    cp -frp /usr/lib64/libpng* /usr/lib/
    cp -frp /usr/lib64/libjpeg* /usr/lib/

参考：http://www.th7.cn/system/lin/201303/37826.shtml


#### B.

因为服务器上没装libmcrypt，所以要先安装一下：

（文件列表：ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt）

    wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt/libmcrypt-2.5.7.tar.gz
    tar zxvf libmcrypt-2.5.7.tar.gz
    cd libmcrypt-2.5.7
    ./configure && make && make install

我这里装了好几次才装好- =。。装到第二次的时候，make出错了，提示

    /usr/bin/ld: cannot find -lltdl
    collect2: ld returned 1 exit status
    make: *** [sapi/cgi/php-cgi] 错误 1

百度了一番，说是--with-mcrypt有问题

于是按解决方案，进入libmcrypt-2.5.7目录下的libtdl把它装一下：

    cd libltdl
    ./configure –enable-ltdl-install
    make
    make install

问题解决

#### C.

    Starting php_fpm Error in argument 1, char 1: no argument for option -
    Usage: php-cgi [-q] [-h] [-s] [-v] [-i] [-f ] php-cgi [args...]
    -a Run interactively
    -C Do not chdir to the script's directory
    -c | Look for php.ini file in this directory
    -n No php.ini file will be used
    -d foo[=bar] Define INI entry foo with value 'bar'
    -e Generate extended information for debugger/profiler
    -f Parse . Implies `-q'
    -h This help -i PHP information
    -l Syntax check only (lint)
    -m Show compiled in modules -q Quiet-mode. Suppress HTTP Header output.
    -s Display colour syntax highlighted source.
    -v Version number
    -w Display source with stripped comments and whitespace.
    -z Load Zend extension
    ................................... failed

在google上找到答案：

因为我安装了好几次（第一次参数错了，第二次配置路径错了），每次重装也没有Clean一下，导致有些已编译的文件一直没重编译，所以很多新参数都没有用上。

make clean一下，然后重新安装。

参考：http://serverfault.com/questions/84926/php-fpm-start-error


#### D.

安装成功后，到/usr/local/php5.2.17下运行sbin/php-fpm start报错：

    [ERROR] fpm_unix_conf_wp(), line 124: please specify user and group other than root, pool 'default'

这是因为php-fpm.conf没有修改好：

默认是<!-- <value name="user">nobody</value> -->

把 <!-- 和 --> 这两个注释符号去掉就行了

按照我的安装路径，php-fpm.conf位于/usr/local/php5.2.17/etc目录下

参考：http://www.360doc.com/content/10/1208/11/4382110_76071592.shtml）


#### E.

配好域名和dns解析后，访问domain/install/一直提示No input file specified.

网上有很多说法，啥document_root什么的有问题了，然后什么php.ini有问题了，各种一大堆

在改nginx配置未果之后，我推测八成是php-fpm没配置好

我登录服务的用户名是online，于是我尝试将php-fpm中刚才去掉注释的那个<value name="user">nobody</value>，把nobody改为online，重启php-fpm

搞定

#### F.

附上nginx配置

    server {
            listen  80;
            server_name your_domain_here.com;
            root /home/online/web/Discuz/upload;
            index index.php;

            if (!-e $request_filename){
                    rewrite ^/(.*) /index.php last;
            }

            client_max_body_size 2M;
            access_log  logs/discuz;
            location ~ .*\.(php)?$ {
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
                #ifastcgi_param   SCRIPT_FILENAME /scripts$fastcgi_script_name;
                include         fastcgi_params;
            }
    }
