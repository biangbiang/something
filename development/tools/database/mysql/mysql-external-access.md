设置mysql允许外网访问
=====================

mysql的root账户,我在连接时通常用的是localhost或127.0.0.1,公司的测试服务器上的mysql也是localhost所以我想访问无法访问,测试暂停.

解决方法如下:

1.修改表,登录mysql数据库,切换到mysql数据库,使用sql语句查看"select host,user from user ;"

    mysql -u root -pvmware
    mysql>use mysql;
    mysql>update user set host = '%' where user ='root';
    mysql>select host, user from user;
    mysql>flush privileges;

注意:最后一句很重要,目的是使修改生效.如果没有写,则还是不能进行远程连接.

2.授权用户,你想root使用密码从任何主机连接到mysql服务器

    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'  IDENTIFIED BY 'admin123'  WITH GRANT OPTION;
    flush privileges;

如果你想允许用户root从ip为192.168.1.104的主机连接到mysql服务器

    GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.1.104'   IDENTIFIED BY 'admin123'  WITH GRANT OPTION; 
    flush privileges;
