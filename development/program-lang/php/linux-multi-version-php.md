linux下nginx多版本php共存
=========================

### 应用环境

LNMP的环境，当前PHP版本5.3.8，遇到一个应用需求只支持PHP 5.2.x，又希望保持现有应用还是用PHP 5.3.8。也就是说需要两个版本的PHP同时存在，供nginx根据需要调用不同版本。

### 思路

Nginx是通过PHP-FastCGI与PHP交互的。而PHP-FastCGI运行后会通过文件、或本地端口两种方式进行监听，在Nginx中配置相应的FastCGI监听端口或文件即实现Nginx请求对PHP的解释。

既然PHP-FastCGI是监听端口和文件的，那就可以让不同版本的PHP-FastCGI同时运行，监听不同的端口或文件，Nginx中根据需求配置调用不同的PHP-FastCGI端口或文件，即可实现不同版本PHP共存了。

### 配置记录

下面记录简单的配置流程，基于已经安装了lnmp的debian环境。当前版本的PHP是5.3.8，位于/usr/local/php。

1. 下载PHP-5.2.14及相关的FPM、autoconf组件：

        mkdir ~/php5.2
        cd ~/php5.2
        wget -c http://museum.php.net/php5/php-5.2.14.tar.gz
        wget -c http://php-fpm.org/downloads/php-5.2.14-fpm-0.5.14.diff.gz

2. 解压PHP-5.2.14，并打上PHP-FPM的补丁：

        tar zxvf php-5.2.14.tar.gz
        gzip -cd php-5.2.14-fpm-0.5.14.diff.gz | patch -d php-5.2.14 -p1

3. 如果你已经通过lnmp安装，应该已经安装好了autoconf，如果没有，请自行下载并编译autoconf-2.13，然后设置autoconf环境变量：

        export PHP_AUTOCONF=/usr/local/autoconf-2.13/bin/autoconf¬
        export PHP_AUTOHEADER=/usr/local/autoconf-2.13/bin/autoheader

4. 编译安装PHP-5.2.14在新的路径（/usr/local/php-5.2.14）下，注意–prefix、–with-config-file-path的路径，并且打开fastcgi和fpm选项：

        cd php-5.2.14/
        ./buildconf --force
        ./configure --prefix=/usr/local/php-5.2.14 --with-config-file-path=/usr/local/php-5.2.14/etc --with-mysql=/usr/local/mysql --with-mysqli=/usr/local/mysql/bin/mysql_config --enable-fastcgi --enable-fpm
        make ZEND_EXTRA_LIBS='-liconv'
        make install

5. 设置/usr/local/php-5.2.14/etc/php-fpm.conf，监听端口：

        <value name="listen_address">127.0.0.1:9001</value>

  或者监听文件：

        <value name="listen_address">/path/to/unix/socket</value>

  其他参数根据服务器环境和需求自行定制。

6. 启动php-fpm，以后可以通过php-fpm进行管理：

        /usr/local/php-5.2.14/sbin/php-fpm start

  自php5.3.3后，php已经将php-fpm继承到php中，而且内置的php-fpm默认不支持(start|stop|reload)的平滑启动参数，需要使用官方源代码中提供的启动脚本来控制：

        cp -f (php -5.3.x-source-dir)/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
        chmod 755 /etc/init.d/php-fpm
        /etc/init.d/php-fpm start

  php-fpm支持的操作：

    * start，启动PHP的FastCGI进程。
    * stop，强制终止PHP的FastCGI进程。
    * quit，平滑终止PHP的FastCGI进程。
    * restart， 重启PHP的FastCGI进程。
    * reload， 重新加载PHP的php.ini。
    * logrotate， 重新启用log文件。

  5.3.3的php-fpm脚本支持的操作：start|stop|force-quit|restart|reload|status

7. 配置好PHP-5.2.14的php.ini，重新加载生效：

        vi /usr/local/php-5.2.14/etc/php.ini
        /usr/local/php-5.2.14/sbin/php-fpm reload

8. 修改nginx配置，对需要的服务配置使用PHP-5.2.14：

        location ~ .*.(php|php5)?$
                {
                    fastcgi_pass  127.0.0.1:9001;
                    fastcgi_index index.php;
                    include fcgi.conf;
                }

9. 记录一下自己编译php5.5.10使用的配置

        ./configure --prefix=/usr/local/php-5.5.10 --with-config-file-path=/usr/local/php-5.5.10/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-bz2 --with-curl=/usr/bin --enable-ftp --enable-sockets --disable-ipv6 --with-gd --with-jpeg-dir=/usr/local --with-png-dir=/usr/local --with-freetype-dir=/usr/local --enable-gd-native-ttf --with-iconv-dir=/usr/local --enable-mbstring --enable-calendar --with-gettext --with-libxml-dir=/usr/local --with-zlib --with-pdo-mysql=mysqlnd --enable-dom --enable-xml --enable-fpm --with-libdir=lib64 --with-mcrypt=/usr/bin --enable-zip --enable-soap --enable-mbstring  --with-gd --with-openssl --enable-pcntl --with-xmlrpc --enable-opcache

### 参考资料：

http://ixdba.blog.51cto.com/2895551/806622

http://www.yinfor.com/blog/archives/2008/05/install\_zend\_optimizer\_33\_on\_c.html

http://ideas.spkcn.com/technology/php-technology/133.html

http://zhangxugg-163-com.iteye.com/blog/1894990
