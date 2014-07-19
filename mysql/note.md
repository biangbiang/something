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

### 外键（Foreign Key）

如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见，外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为主表，具有此外键的表被称为主表的从表。外键又称作外关键字。

通常在数据库设计中缩写为FK。

作用：保持数据一致性，完整性，主要目的是控制存储在外键表中的数据。 使两张表形成关联，外键只能引用外表中的列的值或使用空值。

#### 使用原则

1. 为关联字段创建外键。
2. 所有的键都必须唯一。
3. 避免使用复合键。
4. 外键总是关联唯一的键字段。

### 有效性

有很多时候，程序员会发现字段缺少、多余问题或者是创建外键以后就不能添加没有受约束的行[特殊情况下是有必要的]，这个时候不想对表结构进行操作，就可以使用约束失效。
