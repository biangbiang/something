# mysql explain用法

explain显示了mysql如何使用索引来处理select语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。

使用方法，在select语句前加上explain就可以了，如：

    explain select * from statuses_status where id=11;

![](http://biangbiangpic.b0.upaiyun.com/blog/59e003712c2cbb06a118ba7f8129002e.png)

### explain列的解释

table：显示这一行的数据是关于哪张表的

type：这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为const、eq\_reg、ref、range、indexhe和all

possible\_keys：显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从where语句中选择一个合适的语句

key： 实际使用的索引。如果为null，则没有使用索引。很少的情况下，mysql会选择优化不足的索引。这种情况下，可以在select语句中使用use index（indexname）来强制使用一个索引或者用ignore index（indexname）来强制mysql忽略索引

key\_len：使用的索引的长度。在不损失精确性的情况下，长度越短越好

ref：显示索引的哪一列被使用了，如果可能的话，是一个常数

rows：mysql认为必须检查的用来返回请求数据的行数

extra：关于mysql如何解析查询的额外信息。将在表4.3中讨论，但这里可以看到的坏的例子是using temporary和using filesort，意思mysql根本不能使用索引，结果是检索会很慢

### extra列返回的描述的意义

distinct:一旦mysql找到了与行相联合匹配的行，就不再搜索了

not exists: mysql优化了left join，一旦它找到了匹配left join标准的行，就不再搜索了

range checked for each record（index map:#）:没有找到理想的索引，因此对于从前面表中来的每一个行组合，mysql检查使用哪个索引，并用它来从表中返回行。这是使用索引的最慢的连接之一

using filesort: 看到这个的时候，查询就需要优化了。mysql需要进行额外的步骤来发现如何对返回的行排序。它根据连接类型以及存储排序键值和匹配条件的全部行的行指针来排序全部行

using index: 列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候

using temporary 看到这个的时候，查询需要优化了。这里，mysql需要创建一个临时表来存储结果，这通常发生在对不同的列集进行order by上，而不是group by上

where used 使用了where从句来限制哪些行将与下一张表匹配或者是返回给用户。如果不想返回表中的全部行，并且连接类型all或index，这就会发生，或者是查询有问题不同连接类型的解释（按照效率高低的顺序排序）

system 表只有一行：system表。这是const连接类型的特殊情况

const:表中的一个记录的最大值能够匹配这个查询（索引可以是主键或惟一索引）。因为只有一行，这个值实际就是常数，因为mysql先读这个值然后把它当做常数来对待

eq\_ref:在连接中，mysql在查询时，从前面的表中，对每一个记录的联合都从表中读取一个记录，它在查询使用了索引为主键或惟一键的全部时使用

ref:这个连接类型只有在查询使用了不是惟一或主键的键或者是这些类型的部分（比如，利用最左边前缀）时发生。对于之前的表的每一个行联合，全部记录都将从表中读出。这个类型严重依赖于根据索引匹配的记录多少—越少越好

range:这个连接类型使用索引返回一个范围中的行，比如使用>或<查找东西时发生的情况

index: 这个连接类型对前面的表中的每一个记录联合进行完全扫描（比all更好，因为索引一般小于表数据）

all:这个连接类型对于前面的每一个记录联合进行完全扫描，这一般比较糟糕，应该尽量避免
