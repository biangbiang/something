# 覆盖索引

覆盖索引是select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖。

### 概念

理解方式一：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。一个索引包含了（或覆盖了）满足查询结果的数据就叫做覆盖索引。[1] 

理解方式二：是非聚集复合索引的一种形式，它包括在查询里的Select、Join和Where子句用到的所有列（即建索引的字段正好是覆盖查询条件中所涉及的字段，也即，索引包含了查询正在查找的数据）。

### 作用

如果你想要通过索引覆盖select多列，那么需要给需要的列建立一个多列索引，当然如果带查询条件，where条件要求满足最左前缀原则。

Innodb的辅助索引叶子节点包含的是主键列，所以主键一定是被索引覆盖的。

（1）例如，在sakila的inventory表中，有一个组合索引(store\_id,film\_id)，对于只需要访问这两列的查 询，MySQL就可以使用索引，如下：

    mysql> EXPLAIN SELECT store_id, film_id FROM sakila.inventory\G

（2）再比如说在文章系统里分页显示的时候，一般的查询是这样的：

    SELECT id, title, content FROM article ORDER BY created DESC LIMIT 10000, 10;

通常这样的查询会把索引建在created字段（其中id是主键），不过当LIMIT偏移很大时，查询效率仍然很低，改变一下查询：

    SELECT id, title, content FROM article
    INNER JOIN (
    SELECT id FROM article ORDER BY created DESC LIMIT 10000, 10
    ) AS page USING(id)

此时，建立复合索引"created, id"（只要建立created索引就可以吧，Innodb是会在辅助索引里面存储主键值的），就可以在子查询里利用上Covering Index，快速定位id，查询效率嗷嗷的。
