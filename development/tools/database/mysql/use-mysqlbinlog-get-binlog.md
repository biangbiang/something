使用mysqlbinlog提取二进制日志
=============================

MySQL binlog日志记录了MySQL数据库从启用日志以来所有对当前数据库的变更。binlog日志属于二进制文件，我们可以从binlog提取出来生成可阅读的SQL语句来重建当前数据库以及根据需要实现时点恢复或不完全恢复。本文主要描述了如果提取binlog日志，并给出相关示例。

### 1、提取mysqlbinlog的几种方式

a、使用show binlog events方式可以获取当前以及指定binlog的日志，不适宜提取大量日志。

b、使用mysqlbinlog命令行提取(适宜批量提取日志)。

### 2、演示show binlog events方式

    mysql> show variables like 'version';
    +---------------+------------+
    | Variable_name | Value      |
    +---------------+------------+
    | version      | 5.6.12-log |
    +---------------+------------+

    mysql> show binary logs;
    +-----------------+-----------+
    | Log_name        | File_size |
    +-----------------+-----------+
    | APP01bin.000001 |      120 |
    +-----------------+-----------+

a、只查看第一个binlog文件的内容(show binlog events) 

    mysql> use replication;
    Database changed
    mysql> select * from tb;
    +------+-------+
    | id  | val  |
    +------+-------+
    |    1 | robin |
    +------+-------+

    mysql> insert into tb values(2,'jack');
    Query OK, 1 row affected (0.02 sec)

    mysql> flush logs;
    Query OK, 0 rows affected (0.00 sec)

    mysql> insert into tb values(3,'fred');
    Query OK, 1 row affected (0.00 sec)

    mysql> show binary logs;
    +-----------------+-----------+
    | Log_name        | File_size |
    +-----------------+-----------+
    | APP01bin.000001 |      409 |
    | APP01bin.000002 |      363 |
    +-----------------+-----------+

    mysql> show binlog events;
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+
    | Log_name        | Pos | Event_type  | Server_id | End_log_pos | Info                                              |
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+
    | APP01bin.000001 |  4 | Format_desc |        11 |        120 | Server ver: 5.6.12-log, Binlog ver: 4              |
    | APP01bin.000001 | 120 | Query      |        11 |        213 | BEGIN                                              |
    | APP01bin.000001 | 213 | Query      |        11 |        332 | use `replication`; insert into tb values(2,'jack') |
    | APP01bin.000001 | 332 | Xid        |        11 |        363 | COMMIT /* xid=382 */                              |
    | APP01bin.000001 | 363 | Rotate      |        11 |        409 | APP01bin.000002;pos=4                              |
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+

-- 在上面的结果中第3行可以看到我们执行的SQL语句，第4行为自动提交

b、查看指定binlog文件的内容(show binlog events in 'binname.xxxxx')

    mysql> show binlog events in 'APP01bin.000002';
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+
    | Log_name        | Pos | Event_type  | Server_id | End_log_pos | Info                                              |
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+
    | APP01bin.000002 |  4 | Format_desc |        11 |        120 | Server ver: 5.6.12-log, Binlog ver: 4              |
    | APP01bin.000002 | 120 | Query      |        11 |        213 | BEGIN                                              |
    | APP01bin.000002 | 213 | Query      |        11 |        332 | use `replication`; insert into tb values(3,'fred') |
    | APP01bin.000002 | 332 | Xid        |        11 |        363 | COMMIT /* xid=394 */                              |
    +-----------------+-----+-------------+-----------+-------------+----------------------------------------------------+

c、查看当前正在写入的binlog文件(show master status\G) 

    mysql> show master status\G
    *************************** 1. row ***************************
                File: APP01bin.000002
            Position: 363
        Binlog_Do_DB: 
     Binlog_Ignore_DB: 
    Executed_Gtid_Set: 
    1 row in set (0.00 sec)

d、获取指定位置binlog的内容(show binlog events from) 

    mysql> show binlog events from 213;
    +-----------------+-----+------------+-----------+-------------+----------------------------------------------------+
    | Log_name        | Pos | Event_type | Server_id | End_log_pos | Info                                              |
    +-----------------+-----+------------+-----------+-------------+----------------------------------------------------+
    | APP01bin.000001 | 213 | Query      |        11 |        332 | use `replication`; insert into tb values(2,'jack') |
    | APP01bin.000001 | 332 | Xid        |        11 |        363 | COMMIT /* xid=382 */                              |
    | APP01bin.000001 | 363 | Rotate    |        11 |        409 | APP01bin.000002;pos=4                              |
    +-----------------+-----+------------+-----------+-------------+----------------------------------------------------+

### 3、演示mysqlbinlog方式提取binlog

a、提取指定的binlog日志

    # mysqlbinlog /opt/data/APP01bin.000001
    # mysqlbinlog /opt/data/APP01bin.000001|grep insert
    /*!40019 SET @@session.max_insert_delayed_threads=0*/;
    insert into tb values(2,'jack')

b、提取指定position位置的binlog日志

    # mysqlbinlog --start-position="120" --stop-position="332" /opt/data/APP01bin.000001

c、提取指定position位置的binlog日志并输出到压缩文件

    # mysqlbinlog --start-position="120" --stop-position="332" /opt/data/APP01bin.000001 |gzip >extra_01.sql.gz

d、提取指定position位置的binlog日志导入数据库

    # mysqlbinlog --start-position="120" --stop-position="332" /opt/data/APP01bin.000001 | mysql -uroot -p

e、提取指定开始时间的binlog并输出到日志文件

    # mysqlbinlog --start-datetime="2014-12-15 20:15:23" /opt/data/APP01bin.000002 --result-file=extra02.sql

f、提取指定位置的多个binlog日志文件

    # mysqlbinlog --start-position="120" --stop-position="332" /opt/data/APP01bin.000001 /opt/data/APP01bin.000002|more

g、提取指定数据库binlog并转换字符集到UTF8

    # mysqlbinlog --database=test --set-charset=utf8 /opt/data/APP01bin.000001 /opt/data/APP01bin.000002 >test.sql

h、远程提取日志，指定结束时间 

    # mysqlbinlog -urobin -p -h192.168.1.116 -P3306 --stop-datetime="2014-12-15 20:30:23" --read-from-remote-server mysql-bin.000033 |more

i、远程提取使用row格式的binlog日志并输出到本地文件

    # mysqlbinlog -urobin -p -P3606 -h192.168.1.177 --read-from-remote-server -vv inst3606bin.000005 >row.sql

### 4、获取mysqlbinlog的帮助信息(仅列出常用选项)

    -?, --help
      显示帮助消息并退出。

    -d, --database=name
      只列出该数据库的条目(只适用本地日志)。

    -f, --force-read
      使用该选项，如果mysqlbinlog读它不能识别的二进制日志事件，它会打印警告，忽略该事件并继续。没有该选项，如果mysqlbinlog读到此类事件则停止。

    -h, --host=name
      获取给定主机上的MySQL服务器的二进制日志。

    -l, --local-load=name
      为指定目录中的LOAD DATA INFILE预处理本地临时文件。

    -o, --offset=# 
      跳过前N个条目。

    -p, --password[=name]
      当连接服务器时使用的密码。如果使用短选项形式(-p)，选项和密码之间不能有空格。 
      如果在命令行中–password或-p选项后面没有密码值，则提示输入一个密码。

    -P, --port=# 
      用于连接远程服务器的TCP/IP端口号。

    --protocol=name
      使用的连接协议。

    -R, --read-from-remote-server|--read-from-remote-master=name
      从MySQL服务器读二进制日志。如果未给出该选项，任何连接参数选项将被忽略，即连接到本地。
      这些选项是–host、–password、–port、–protocol、–socket和–user。

    -r, --result-file=name 
      将输出指向给定的文件。

    -s, --short-form
      只显示日志中包含的语句，不显示其它信息，该方式可以缩小生成sql文件的尺寸。

    -S, --socket=name
      用于连接的套接字文件。

    --start-datetime=name
      从二进制日志中读取等于或晚于datetime参量的事件，datetime值相对于运行mysqlbinlog的机器上的本地时区。
      该值格式应符合DATETIME或TIMESTAMP数据类型。例如：2004-12-25 11:25:56 ，建议使用引号标识。

    --stop-datetime=name
      从二进制日志中读取小于或等于datetime的所有日志事件。关于datetime值的描述参见--start-datetime选项。

    -j, --start-position=# 
      从二进制日志中第1个位置等于N参量时的事件开始读。

    --stop-position=#
      从二进制日志中第1个位置等于和大于N参量时的事件起停止读。

    --server-id=#  
      仅仅提取指定server_id的binlog日志

    --set-charset=name 
      添加SET NAMES character_set到输出      
                    
    -t, --to-last-log 
      在MySQL服务器中请求的二进制日志的结尾处不停止，而是继续打印直到最后一个二进制日志的结尾。
      如果将输出发送给同一台MySQL服务器，会导致无限循环。该选项要求–read-from-remote-server。

    -D, --disable-log-bin
      禁用二进制日志。如果使用–to-last-logs选项将输出发送给同一台MySQL服务器，可以避免无限循环。
      该选项在崩溃恢复时也很有用，可以避免复制已经记录的语句。注释：该选项要求有SUPER权限。

    -u, --user=name
      连接远程服务器时使用的MySQL用户名。

    -v, --verbose
      用于输出基于row模式的binlog日志，-vv为列数据类型添加注释

    -V, --version
      显示版本信息并退出。

### 5、小结

a、可以通过show binlog events以及mysqlbinlog方式来提取binlog日志。

b、show binlog events 参数有限不适宜批量提取，mysqlbinlog可用于批量提取来建立恢复数据库的SQL。

c、mysqlbinlog可以基于时间点，position等方式实现不完全恢复或时点恢复。

d、mysqlbinlog可以从支持本地或远程方式提取binlog日志。

e、mysqlbinlog可以基于server_id，以及基于数据库级别提取日志，不支持表级别。
