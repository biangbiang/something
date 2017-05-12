# varchar和text说不清的那些事

最近有几个同学问我varchar和text有啥别吗，这个问题，以前说真的也没太多的整理，以前遇到text在设计中就是尽可能的拆到另一个表中，保持主表尽量的瘦小，可以让innodb bp缓存更多的数据。

今天借次机会系统整理一下，主要从存储上，最大值，默认值几个方面进行比较。

BTW： 从ISO SQL:2003上讲VARCHAR是一个标准型，但TEXT不是（包括tinytext）.varchar在MySQL 5.0.3之前只支持0-255byte, 在5.0.3之后才支持到0-65535byte.

从存储上讲：

- text 是要要进overflow存储。 也是对于text字段，不会和行数据存在一起。但原则上不会全部overflow ,会有768字节和原始的行存储在一块，多于768的行会存在和行相同的Page或是其它Page上。
  
- varchar 在MySQL内部属于从blob发展出来的一个结构，在早期版本中innobase中，也是768字节以后进行overfolw存储。
  
- 对于Innodb-plugin后： 对于变长字段处理都是20Byte后进行overflow存储（在新的row_format下：dynimic compress）

说完存储后，说一下使用这些大的变长字段的缺点：

- 在Innobase中,变长字段，是尽可能的存储到一个Page里，这样，如果使用到这些大的变长字段，会造成一个Page里能容纳的行数很少，在查询时，虽然没查询这些大的字段，但也会加载到innodb buffer pool中，等于浪费的内存。（buffer pool 的缓存是按page为单位）（不在一个page了会增加随机的IO）
 
- 在innodb-plugin中为了减少这种大的变长字段对内存的浪费，引入了大于20个字节的，都进行overflow存储，而且希望不要存到相同的page中，为了增加一个page里能存储更多的行，提高buffer pool的利用率。 这也要求我们，如果不是特别需要就不要读取那些变长的字段。


那问题来了？ 为什么varchar(255+)存储上和text很相似了，但为什么还要有varchar, mediumtext, text这些类型？

（从存储上来讲大于255的varchar可以说是转换成了text.这也是为什么varchar大于65535了会转成mediumtext)

我理解：这块是一方面的兼容，另一方面在非空的默认值上varchar和text有区别。从整体上看功能上还是差别的。

这里还涉及到字段额外开销的：

- varchar 小于255byte  1byte overhead
- varchar 大于255byte  2byte overhead
 
- tinytext 0-255 1 byte overhead
- text 0-65535 byte 2 byte overhead
- mediumtext 0-16M  3 byte overhead
 
- longtext 0-4Gb 4byte overhead

备注 overhead是指需要几个字节用于记录该字段的实际长度。

从处理形态上来讲varchar 大于768字节后，实质上存储和text差别不是太大了。 基本认为是一样的。

另外从8000byte这个点说明一下： 对于varcahr, text如果行不超过8000byte（大约的数，innodb data page的一半） ,overflow不会存到别的page中。基于上面的特性可以总结为text只是一个MySQL扩展出来的特殊语法有兼容的感觉。

默认值问题：

- 对于text字段，MySQL不允许有默认值。
- varchar允许有默认值

### 总结：

根据存储的实现： 可以考虑用varchar替代tinytext

如果需要非空的默认值，就必须使用varchar

如果存储的数据大于64K，就必须使用到mediumtext , longtext

varchar(255+)和text在存储机制是一样的
 
需要特别注意varchar(255)不只是255byte ,实质上有可能占用的更多。
 
特别注意，varchar大字段一样的会降低性能，所以在设计中还是一个原则大字段要拆出去，主表还是要尽量的瘦小

源码中类型：

    +--Field_str (abstract)
     |  +--Field_longstr
     |  |  +--Field_string
     |  |  +--Field_varstring
     |  |  +--Field_blob
     |  |     +--Field_geom
     |  |
     |  +--Field_null
     |  +--Field_enum
     |     +--Field_set

（末完待续，也希望大家一块讨论一下）

### 参考：

http://yoshinorimatsunobu.blogspot.com/2010/11/handling-long-textsblobs-in-innodb-1-to.html

http://nicj.net/mysql-text-vs-varchar-performance/

http://www.pythian.com/blog/text-vs-varchar/

### 测试SQL及方法

    create table tb_01(
    c1 varchar(255),
    c2 varchar(255),
    c3 varchar(255),
    c4 varchar(255),
    c5 varchar(255),
    c6 varchar(255),
    c7 varchar(255),
    c8 varchar(255),
    c9 varchar(255),
    c10 varchar(255),
    c11 varchar(255)
    )engine=Innodb;
     
    insert into tb_01(c1,c2,c3,c4,c5,c6,c7,c8,c9,c10,c11) values(repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255));
    ERROR 1118 (42000): Row size too large (&gt; 8126). Changing some columns to TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED may help. In current row format, BLOB prefix of 768 bytes is stored inline.
     
    (testing)root@localhost [wubx]&gt; set global innodb_file_format=BARRACUDA;
    Query OK, 0 rows affected (0.00 sec)
     
    (testing)root@localhost [wubx]&gt; alter table tb_01 row_format=dynamic;
    Query OK, 0 rows affected (0.19 sec)
    Records: 0  Duplicates: 0  Warnings: 0
     
    (testing)root@localhost [wubx]&gt; insert into tb_01(c1,c2,c3,c4,c5,c6,c7,c8,c9,c10,c11) values(repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255));
    Query OK, 1 row affected (0.00 sec)
     
     
    set global innodb_file_format=Antelope;
    create table tb_02(
    c1 varchar(2000),
    c2 varchar(2000),
    c3 varchar(2000),
    c4 varchar(2000),
    c5 varchar(2000),
    c6 varchar(2000),
    c7 varchar(2000),
    c8 varchar(2000)
    )engine=Innodb;
     
    insert into tb_02(c1, c2, c3,c4,c5,c6,c7,c8) values(repeat('吴',2000),repeat('吴',2000),repeat('吴',2000),repeat('吴',2000),repeat('吴',2000),repeat('吴',2000),repeat('吴',2000),repeat('吴',2000) );
     
     
    create table tb_03(
    c1 text,
    c2 text,
    c3 text,
    c4 text,
    c5 text,
    c6 text,
    c7 text,
    c8 text,
    c9 text,
    c10 text,
    c11 text
    )engine=Innodb;
    insert into tb_03(c1,c2,c3,c4,c5,c6,c7,c8,c9,c10,c11) values(repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255));
     
    (testing)root@localhost [wubx]&gt; insert into tb_03(c1,c2,c3,c4,c5,c6,c7,c8,c9,c10,c11) values(repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255),repeat('吴',255));
    ERROR 1118 (42000): Row size too large (&gt; 8126). Changing some columns to TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED may help. In current row format, BLOB prefix of 768 bytes is stored inline.
     
    set global innodb_file_format=BARRACUDA;
    alter table tb_03 row_format=dynamic;
