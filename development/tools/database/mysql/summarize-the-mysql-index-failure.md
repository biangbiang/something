# MYSQL索引失效的各种情形总结

1) 没有查询条件，或者查询条件没有建立索引 

2) 在查询条件上没有使用引导列 

3) 查询的数量是大表的大部分，应该是30％以上。 

4) 索引本身失效

5) 查询条件使用函数在索引列上，或者对索引列进行运算，运算包括(+，-，*，/，! 等) 

错误的例子：

    select * from test where id-1=9; 正确的例子：select * from test where id=10; 

6) 对小表查询 

7) 提示不使用索引

8) 统计数据不真实 

9) CBO计算走索引花费过大的情况。其实也包含了上面的情况，这里指的是表占有的block要比索引小。 

10) 隐式转换导致索引失效.这一点应当引起重视.也是开发中经常会犯的错误. 由于表的字段tu\_mdn定义为varchar2(20),但在查询时把该字段作为number类型以where条件传给Oracle,这样会导致索引失效. 

错误的例子：

    select * from test where tu_mdn=13333333333; 

正确的例子：

    select * from test where tu_mdn='13333333333'; 

12) 1,<> 2,单独的>,<,(有时会用到，有时不会) 

13) like "%_" 百分号在前. 

14) 表没分析. 

15) 单独引用复合索引里非第一位置的索引列. 

16) 字符型字段为数字时在where条件里不添加引号. 

17) 对索引列进行运算.需要建立函数索引. 

18) not in ,not exist. 

19) 当变量采用的是times变量，而表的字段采用的是date变量时.或相反情况。 

20) B-tree索引 is null不会走,is not null会走,位图索引 is null,is not null 都会走 

21) 联合索引 is not null 只要在建立的索引列（不分先后）都会走, in null时 必须要和建立索引第一列一起使用,当建立索引第一位置条件是is null 时,其他建立索引的列可以是is null（但必须在所有列 都满足is null的时候）,或者=一个值； 当建立索引的第一位置是=一个值时,其他索引列可以是任何情况（包括is null =一个值）,以上两种情况索引都会走。其他情况不会走。