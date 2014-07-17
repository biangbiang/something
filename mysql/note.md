mysql学习笔记
===========

### 排序

* asc：正序排序，即从小到大排序
* desc：逆序排序，即从大到小排序

### 新增用户访问

在mysql的user表中增加连接用户帐号

    GRANT USAGE ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;

可访问数据表授权

    GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON tablename.*  TO 'username'@'localhost' IDENTIFIED BY 'password';
