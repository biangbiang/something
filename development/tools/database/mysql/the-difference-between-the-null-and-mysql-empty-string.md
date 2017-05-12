# NULL与MySQL空字符串的区别

MySQL空字符串和NULL值我们都经常会见到，但是这二者并不是一个概念，下面就为您介绍NULL与MySQL空字符串的区别，供您参考。

对于SQL的新手，NULL值的概念常常会造成混淆，他们常认为NULL与MySQL空字符串是相同的事。情况并非如此。例如，下述语句是完全不同的：

    MySQL> INSERT INTO my_table (phone) VALUES (NULL);
    mysql> INSERT INTO my_table (phone) VALUES ('');   

这两条语句均会将值插入phone（电话）列，但第1条语句插入的是NULL值，第2条语句插入的是空字符串。第1种情况的含义可被解释为“电话号码未知”，而第2种情况的含义可被解释为“该人员没有电话，因此没有电话号码”。

为了进行NULL处理，可使用IS NULL和IS NOT NULL操作符以及IFNULL()函数。

在SQL中，NULL值与任何其它值的比较（即使是NULL）永远不会为“真”。包含NULL的表达式总是会导出NULL值，除非在关于操作符的文 档中以及表达式的函数中作了其他规定。下述示例中的所有列均返回NULL：mysql> SELECT NULL, 1+NULL, CONCAT('Invisible',NULL);   

如果打算搜索列值为NULL的列，不能使用expr = NULL测试。下述语句不返回任何行，这是因为，对于任何表达式，expr = NULL永远不为“真”： mysql> SELECT * FROM my_table WHERE phone = NULL;   

要想查找NULL值，必须使用IS NULL测试。在下面的语句中，介绍了查找NULL电话号码和空电话号码的方式：mysql> SELECT * FROM my_table WHERE phone IS NULL;

    mysql> SELECT * FROM my_table WHERE phone = '';   

### 更多信息和示例：

如果你正在使用MyISAM、InnoDB、BDB、或MEMORY存储引擎，能够在可能具有NULL值的列上增加1条索引。如不然，必须声明索引列为NOT NULL，而且不能将NULL插入到列中。

用LOAD DATA INFILE读取数据时，对于空的或丢失的列，将用''更新它们。如果希望在列中具有NULL值，应在数据文件中使用\N。在某些情况下，也可以使用文字性单词“NULL”。

使用DISTINCT、GROUP BY或ORDER BY时，所有NULL值将被视为等同的。

使用ORDER BY时，首先将显示NULL值，如果指定了DESC按降序排列，NULL值将最后显示。

对于聚合（累计）函数，如COUNT()、MIN()和SUM()，将忽略NULL值。对此的例外是COUNT(\*)，它将计数行而不是单独的列 值。

例如，下述语句产生两个计数。首先计数表中的行数，其次计数age列中的非NULL值数目：

    mysql> SELECT COUNT(*), COUNT(age) FROM person;   

对于某些列类型，MySQL将对NULL值进行特殊处理。如果将NULL插入TIMESTAMP列，将插入当前日期和时间。如果将NULL插入具有AUTO_INCREMENT属性的整数列，将插入序列中的下一个编号。
