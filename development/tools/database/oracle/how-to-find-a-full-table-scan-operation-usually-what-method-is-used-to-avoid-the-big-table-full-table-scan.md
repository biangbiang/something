# 如何发现全表扫描操作?通常采用哪些方法避免大表全表扫描?  

### 如何发现全表扫描语句大量占用CPU资源？

unix平台：

1. 查看CPU利用情况： vmstat 1

2. 查看哪个进程占用CPU资源最多： top

3. 查看oracle进程数量：ps -fe|grep ora 

4. 查看oracle等待事件：

        select sid,event,p1,p1text from v$session_wait;

  可能会发现存在大量db file scattered read及db file sequential read等待。

5. 捕获相关SQL

        select sql_text
        from v$sqltext a
        where a.hash_value = (
        select sql_hash_value from v$session b
        where b.sid='&sid'
        )
        order by piece asc
        /

6. 得到sql语句之后，查询此语句执行计划

        SQL>set autotrace trace explain
        SQL> xxxx             --此语句。

  会发现用了全表扫描。

7. 查看此表相关index键。

        select index_name,column_name from user_ind_columns  where table_name ='xxx';

  创建新index以消除全表扫描。

8. 再查看此语句执行计划。。

9. 确认CPU利用情况，oracle等待事件情况。

### 会引起全表扫描的几种SQL

1、模糊查询效率很低：

原因：like本身效率就比较低，应该尽量避免查询条件使用like；对于like ‘%...%’（全模糊）这样的条件，是无法使用索引的，全表扫描自然效率很低；另外，由于匹配算法的关系，模糊查询的字段长度越大，模糊查询效率越低。

解决办法：首先尽量避免模糊查询，如果因为业务需要一定要使用模糊查询，则至少保证不要使用全模糊查询，对于右模糊查询，即like ‘…%’，是会使用索引的；左模糊like

‘%...’无法直接使用索引，但可以利用reverse + function index 的形式，变化成 like ‘…%’；全模糊是无法优化的，一定要的话考虑用搜索引擎。出于降低数据库服务器的负载考虑，尽可能地减少数据库模糊查询。

2、查询条件中含有is null的select语句执行慢

原因：Oracle 9i中，查询字段is null时单索引失效，引起全表扫描。

解决方法：SQL语法中使用NULL会有很多麻烦，最好索引列都是NOT NULL的；对于is null，可以建立组合索引，nvl(字段,0),对表和索引analyse后，is null查询时可以重新启用索引查找,但是效率还不是值得肯定；is not null 时永远不会使用索引。一般数据量大的表不要用is null查询。

3、查询条件中使用了不等于操作符（<>、!=）的select语句执行慢

原因：SQL中，不等于操作符会限制索引，引起全表扫描，即使比较的字段上有索引

解决方法：通过把不等于操作符改成or，可以使用索引，避免全表扫描。例如，把column<>’aaa’，改成column<’aaa’ or column>’aaa’，就可以使用索引了。

4、使用组合索引，如果查询条件中没有前导列，那么索引不起作用，会引起全表扫描；但是从Oracle9i开始，引入了索引跳跃式扫描的特性，可以允许优化器使用组合索引，即便索引的前导列没有出现在WHERE子句中。

例如：

    create index skip1 on emp5(job,empno);   

全索引扫描 

    select count(*) from emp5 where empno=7900;   

索引跳跃式扫描 

    select /*+ index(emp5 skip1)*/ count(*) from emp5 where empno=7900; 

前一种是全表扫描，后一种则会使用组合索引。

5、or语句使用不当会引起全表扫描

原因：where子句中比较的两个条件，一个有索引，一个没索引，使用or则会引起全表扫描。例如：where A=：1 or B=：2，A上有索引，B上没索引，则比较B=：2时会重新开始全表扫描。

6、组合索引，排序时应按照组合索引中各列的顺序进行排序，即使索引中只有一个列是要排序的，否则排序性能会比较差。

例如：

    create index skip1 on emp5(job,empno，date);  
    select job，empno from emp5 where job=’manager’and empno=’10’ order by job,empno,date desc; 

实际上只是查询出符合job=’manager’and empno=’10’条件的记录并按date降序排列，但是写成order by date desc性能较差。

7、Update 语句，如果只更改1、2个字段，不要Update全部字段，否则频繁调用会引起明显的性能消耗，同时带来大量日志。

8、对于多张大数据量（这里几百条就算大了）的表JOIN，要先分页再JOIN，否则逻辑读会很高，性能很差。

9、`select count(*) from table；`这样不带任何条件的count会引起全表扫描，并且没有任何业务意义，是一定要杜绝的。

10、sql的where条件要绑定变量，比如where column=：1，不要写成where column=‘aaa’，这样会导致每次执行时都会重新分析，浪费CPU和内存资源。
