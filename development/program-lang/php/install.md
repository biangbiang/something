php环境搭建
=========

## 安装扩展

### 生成动态链接库文件.so

* 方法1：apt-get install {name}
* 方法2：去PHP官网下载tar包，phpize本地编译生成.so
* 方法3：pear方式安装，通过pecl命令去在线下载编译生成.so

### 配置php.ini

打开php.ini，指定extension_dir目录，如果extension_dir = '/usr/lib',那么接下来把生成的.so文件（如curl.so）复制到/usr/lib目录下，并且加入一个新条目：
extension=curl.so

### 使之生效

重新启动相关软件，运行phpinfo()看是否生效
