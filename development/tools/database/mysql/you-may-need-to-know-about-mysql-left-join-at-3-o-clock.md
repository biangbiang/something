# 关于 MySQL LEFT JOIN 你可能需要了解的三点

即使你认为自己已对 MySQL 的 LEFT JOIN 理解深刻，但我敢打赌，这篇文章肯定能让你学会点东西！

### ON 子句与 WHERE 子句的不同

一种更好地理解带有 WHERE ... IS NULL 子句的复杂匹配条件的简单方法 

Matching-Conditions 与 Where-conditions 的不同

关于 “A LEFT JOIN B ON 条件表达式” 的一点提醒

ON 条件（“A LEFT JOIN B ON 条件表达式”中的ON）用来决定如何从 B 表中检索数据行。

如果 B 表中没有任何一行数据匹配 ON 的条件,将会额外生成一行所有列为 NULL 的数据

在匹配阶段 WHERE 子句的条件都不会被使用。仅在匹配阶段完成以后，WHERE 子句条件才会被使用。它将从匹配阶段产生的数据中检索过滤。

让我们看一个 LFET JOIN 示例：

    mysql> CREATE TABLE `product` (
      `id` int(10) unsigned NOT NULL auto_increment,
      `amount` int(10) unsigned default NULL,
      PRIMARY KEY  (`id`)
    ) ENGINE=MyISAM AUTO_INCREMENT=5 DEFAULT CHARSET=latin1
     
    mysql> CREATE TABLE `product_details` (
      `id` int(10) unsigned NOT NULL,
      `weight` int(10) unsigned default NULL,
      `exist` int(10) unsigned default NULL,
      PRIMARY KEY  (`id`)
    ) ENGINE=MyISAM DEFAULT CHARSET=latin1
     
    mysql> INSERT INTO product (id,amount)
           VALUES (1,100),(2,200),(3,300),(4,400);
    Query OK, 4 rows affected (0.00 sec)
    Records: 4  Duplicates: 0  Warnings: 0
     
    mysql> INSERT INTO product_details (id,weight,exist)
           VALUES (2,22,0),(4,44,1),(5,55,0),(6,66,1);
    Query OK, 4 rows affected (0.00 sec)
    Records: 4  Duplicates: 0  Warnings: 0
     
    mysql> SELECT * FROM product;
    +----+--------+
    | id | amount |
    +----+--------+
    |  1 |    100 |
    |  2 |    200 |
    |  3 |    300 |
    |  4 |    400 |
    +----+--------+
    4 rows in set (0.00 sec)
     
    mysql> SELECT * FROM product_details;
    +----+--------+-------+
    | id | weight | exist |
    +----+--------+-------+
    |  2 |     22 |     0 |
    |  4 |     44 |     1 |
    |  5 |     55 |     0 |
    |  6 |     66 |     1 |
    +----+--------+-------+
    4 rows in set (0.00 sec)
     
    mysql> SELECT * FROM product LEFT JOIN product_details
           ON (product.id = product_details.id);
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 |    2 |     22 |     0 |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 |    4 |     44 |     1 |
    +----+--------+------+--------+-------+
    4 rows in set (0.00 sec)

### ON 子句和 WHERE 子句有什么不同？

一个问题：下面两个查询的结果集有什么不同么？

    1. SELECT * FROM product LEFT JOIN product_details
             ON (product.id = product_details.id)
             AND   product_details.id=2;
    2. SELECT * FROM product LEFT JOIN product_details
             ON (product.id = product_details.id)
             WHERE product_details.id=2;

用例子来理解最好不过了：

    mysql> SELECT * FROM product LEFT JOIN product_details
           ON (product.id = product_details.id)
           AND product_details.id=2;
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 |    2 |     22 |     0 |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 | NULL |   NULL |  NULL |
    +----+--------+------+--------+-------+
    4 rows in set (0.00 sec)
     
    mysql> SELECT * FROM product LEFT JOIN product_details
           ON (product.id = product_details.id)
           WHERE product_details.id=2;
    +----+--------+----+--------+-------+
    | id | amount | id | weight | exist |
    +----+--------+----+--------+-------+
    |  2 |    200 |  2 |     22 |     0 |
    +----+--------+----+--------+-------+
    1 row in set (0.01 sec)

第一条查询使用 ON 条件决定了从 LEFT JOIN的 product\_details表中检索符合的所有数据行。

第二条查询做了简单的LEFT JOIN，然后使用 WHERE 子句从 LEFT JOIN的数据中过滤掉不符合条件的数据行。

再来看一些示例：

    mysql>
    mysql> SELECT * FROM product LEFT JOIN product_details
           ON product.id = product_details.id
           AND product.amount=100;
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 | NULL |   NULL |  NULL |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 | NULL |   NULL |  NULL |
    +----+--------+------+--------+-------+
    4 rows in set (0.00 sec)

所有来自product表的数据行都被检索到了，但没有在product_details表中匹配到记录（product.id = product_details.id AND product.amount=100 条件并没有匹配到任何数据）

    mysql> SELECT * FROM product LEFT JOIN product_details
           ON (product.id = product_details.id)
           AND product.amount=200;
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 |    2 |     22 |     0 |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 | NULL |   NULL |  NULL |
    +----+--------+------+--------+-------+
    4 rows in set (0.01 sec)

同样，所有来自product表的数据行都被检索到了，有一条数据匹配到了。

### 使用 WHERE ... IS NULL 子句的 LEFT JOIN

当你使用 WHERE ... IS NULL 子句时会发生什么呢？

如前所述，WHERE 条件查询发生在 匹配阶段之后，这意味着 WHERE ... IS NULL 子句将从匹配阶段后的数据中过滤掉不满足匹配条件的数据行。

纸面上看起来很清楚，但是当你在 ON 子句中使用多个条件时就会感到困惑了。

我总结了一种简单的方式来理解上述情况：

将 IS NULL 作为否定匹配条件

使用 !(A and B) == !A OR !B 逻辑判断

看看下面的示例：

    mysql> SELECT a.* FROM product a LEFT JOIN product_details b
           ON a.id=b.id AND b.weight!=44 AND b.exist=0
           WHERE b.id IS NULL;
    +----+--------+
    | id | amount |
    +----+--------+
    |  1 |    100 |
    |  3 |    300 |
    |  4 |    400 |
    +----+--------+
    3 rows in set (0.00 sec)

让我们检查一下 ON 匹配子句：

    (a.id=b.id) AND (b.weight!=44) AND (b.exist=0)

我们可以把 IS NULL 子句 看作是否定匹配条件。

这意味着我们将检索到以下行：

    !( exist(b.id that equals to a.id) AND b.weight !=44 AND b.exist=0 )
    !exist(b.id that equals to a.id) || !(b.weight !=44) || !(b.exist=0)
    !exist(b.id that equals to a.id) || b.weight =44 || b.exist=1

就像在C语言中的逻辑 AND 和 逻辑 OR表达式一样，其操作数是从左到右求值的。如果第一个参数做够判断操作结果，那么第二个参数便不会被计算求值（短路效果）

看看别的示例：

    mysql> SELECT a.* FROM product a LEFT JOIN product_details b
           ON a.id=b.id AND b.weight!=44 AND b.exist=1
           WHERE b.id IS NULL;
    +----+--------+
    | id | amount |
    +----+--------+
    |  1 |    100 |
    |  2 |    200 |
    |  3 |    300 |
    |  4 |    400 |
    +----+--------+
    4 rows in set (0.00 sec)

Matching-Conditions 与 Where-conditions 之战

如果你吧基本的查询条件放在 ON 子句中，把剩下的否定条件放在 WHERE 子句中，那么你会获得相同的结果。

例如，你可以不这样写：

    SELECT a.* FROM product a LEFT JOIN product_details b
    ON a.id=b.id AND b.weight!=44 AND b.exist=0
    WHERE b.id IS NULL;

你可以这样写：

    SELECT a.* FROM product a LEFT JOIN product_details b
    ON a.id=b.id
    WHERE b.id is null OR b.weight=44 OR b.exist=1;
    mysql> SELECT a.* FROM product a LEFT JOIN product_details b
           ON a.id=b.id
           WHERE b.id is null OR b.weight=44 OR b.exist=1;
    +----+--------+
    | id | amount |
    +----+--------+
    |  1 |    100 |
    |  3 |    300 |
    |  4 |    400 |
    +----+--------+
    3 rows in set (0.00 sec)

你可以不这样写：

    SELECT a.* FROM product a LEFT JOIN product_details b
    ON a.id=b.id AND b.weight!=44 AND b.exist!=0
    WHERE b.id IS NULL;

可以这样写：

    SELECT a.* FROM product a LEFT JOIN product_details b
    ON a.id=b.id
    WHERE b.id is null OR b.weight=44 OR b.exist=0;
    mysql> SELECT a.* FROM product a LEFT JOIN product_details b
           ON a.id=b.id
           WHERE b.id is null OR b.weight=44 OR b.exist=0;
    +----+--------+
    | id | amount |
    +----+--------+
    |  1 |    100 |
    |  2 |    200 |
    |  3 |    300 |
    |  4 |    400 |
    +----+--------+
    4 rows in set (0.00 sec)

这些查询真的效果一样？

如果你只需要第一个表中的数据的话，这些查询会返回相同的结果集。有一种情况就是，如果你从 LEFT JOIN的表中检索数据时，查询的结果就不同了。

如前所属，WHERE 子句是在匹配阶段之后用来过滤的。

例如：

    mysql> SELECT * FROM product a LEFT JOIN product_details b
           ON a.id=b.id AND b.weight!=44 AND b.exist=1
           WHERE b.id is null;
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 | NULL |   NULL |  NULL |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 | NULL |   NULL |  NULL |
    +----+--------+------+--------+-------+
    4 rows in set (0.00 sec)
     
    mysql> SELECT * FROM product a LEFT JOIN product_details b
           ON a.id=b.id
           WHERE b.id IS NULL OR b.weight=44 OR b.exist=0;
    +----+--------+------+--------+-------+
    | id | amount | id   | weight | exist |
    +----+--------+------+--------+-------+
    |  1 |    100 | NULL |   NULL |  NULL |
    |  2 |    200 |    2 |     22 |     0 |
    |  3 |    300 | NULL |   NULL |  NULL |
    |  4 |    400 |    4 |     44 |     1 |
    +----+--------+------+--------+-------+
    4 rows in set (0.00 sec)

总附注：

如果你使用 LEFT JOIN 来寻找在一些表中不存在的记录，你需要做下面的测试：WHERE 部分的 col\_name IS NULL(其中 col\_name 列被定义为 NOT NULL)，MYSQL 在查询到一条匹配 LEFT JOIN 条件后将停止搜索更多行（在一个特定的组合键下）。
