# mysql 联合索引的意义

问题？

因为什么需求，要创建‘联合索引’？最实际好处在于什么？如果是为了更快查询到数据，有单列索引不是Ok？为什么有‘联合索引’的存在？

### 一

简单的说有两个主要原因：

* "一个顶三个"。建了一个(a,b,c)的复合索引，那么实际等于建了(a),(a,b),(a,b,c)三个索引，因为每多一个索引，都会增加写操作的开销和磁盘空间的开销。对于大量数据的表，这可是不小的开销！

* 覆盖索引。同样的有复合索引（a,b,c），如果有如下的sql: select a,b,c from table where a=1 and b = 1。那么MySQL可以直接通过遍历索引取得数据，而无需回表，这减少了很多的随机io操作。减少io操作，特别的随机io其实是dba主要的优化策略。所以，在真正的实际应用中，覆盖索引是主要的提升性能的优化手段之一

* 索引列越多，通过索引筛选出的数据越少。有1000W条数据的表，有如下sql:select from table where a = 1 and b =2 and c = 3,假设假设每个条件可以筛选出10%的数据，如果只有单值索引，那么通过该索引能筛选出1000W10%=100w 条数据，然后再回表从100w条数据中找到符合b=2 and c= 3的数据，然后再排序，再分页；如果是复合索引，通过索引筛选出1000w 10% 10% *10%=1w，然后再排序、分页，哪个更高效，一眼便知

### 二

如下的有a,b,c 三个key的table

    create table test(
        a int,
        b int,
        c int,
    );

如果我们

需要执行很多的类似于 select * from test where a=10, b>50, c>20

这类的组合查询 那么，我们可能需要创建 包含[a，b，c] 的联合索引，而单独的[a][b] [c]上的索引是不够的。（可以把一个索引想象成 sorted list）.创建了 （a，b，c）的索引相当于 按照a,b,c 排序（排序规则是

    if(X.a>Y.a)
        return '>';
    else if(X.a<Y.a)
        return '<';
    else if(X.b>Y.b)
        return '>';
    else if (X.b<Y.b)
        return '<';
    else if (X.c>Y.c)
        return  '>'
    else if (X.c<Y.c)
        return  '<'
    esle
        return '=='

）和分别 按a 排序 分别按b排序 分别按照c排序是不一样的。

其中 a b c 的顺序也很重要，有时可以是a c b，或者b c a等等。

如果创建 （a,b,c)的联合索引，查询效率如下：

    优: select * from test where a=10 and b>50
    差: select * from test where a>50

    优: select * from test order by a
    差: select * from test order by b
    差: select * from test order by c

    优: select * from test where a=10 order by a
    优: select * from test where a=10 order by b
    差: select * from test where a=10 order by c

    优: select * from test where a>10 order by a
    差: select * from test where a>10 order by b
    差: select * from test where a>10 order by c

    优: select * from test where a=10 and b=10 order by a
    优: select * from test where a=10 and b=10 order by b
    优: select * from test where a=10 and b=10 order by c

    优: select * from test where a=10 and b=10 order by a
    优: select * from test where a=10 and b>10 order by b
    差: select * from test where a=10 and b>10 order by c

下面用图示的方式来表示

![](http://biang.io/biangpic/blog/24ffcac065491bf9944a029ba261e0e6.jpg)

### 三 注意

在mysql中 若列是varchar 类型，请不要使用 int类型去访问

如下

zz_deals表中 product_id 是varchar类型

    mysql> explain select * from zz_deals where qq_shop_id = 64230 and product_id = '38605906667' ;
    +----+-------------+-------------+------+------------------------------+------------------------------+---------+-------------+------+-------------+
    | id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra |
    +----+-------------+-------------+------+------------------------------+------------------------------+---------+-------------+------+-------------+
    | 1 | SIMPLE | zz_deals | ref | by_product_id_and_qq_shop_id | by_product_id_and_qq_shop_id | 156 | const,const | 1 | Using where |
    +----+-------------+-------------+------+------------------------------+------------------------------+---------+-------------+------+-------------+
    1 row in set (0.00 sec)

    mysql> explain select * from zz_deals where qq_shop_id = 64230 and product_id = 38605906667 ;
    +----+-------------+-------------+------+------------------------------+------+---------+------+------+-------------+
    | id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra |
    +----+-------------+-------------+------+------------------------------+------+---------+------+------+-------------+
    | 1 | SIMPLE | zz_deals | ALL | by_product_id_and_qq_shop_id | NULL | NULL | NULL | 17 | Using where |
    +----+-------------+-------------+------+------------------------------+------+---------+------+------+-------------+
    1 row in set (0.00 sec)
