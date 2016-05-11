MySQL 二进制日志(Binary Log)
=============================

同大多数关系型数据库一样，日志文件是MySQL数据库的重要组成部分。MySQL有几种不同的日志文件，通常包括错误日志文件，二进制日志，通用日志，慢查询日志，等等。这些日志可以帮助我们定位mysqld内部发生的事件，数据库性能故障，记录数据的变更历史，用户恢复数据库等等。二进制日志，也叫binary log，是MySQL Server中最为重要的日志之一，本文主要描述二进制日志。

---

### 1、MySQL日志文件系统的组成

* a、错误日志：记录启动、运行或停止mysqld时出现的问题。
* b、通用日志：记录建立的客户端连接和执行的语句。
* c、更新日志：记录更改数据的语句。该日志在MySQL 5.1中已不再使用。
* d、二进制日志：记录所有更改数据的语句。还用于复制。
* e、慢查询日志：记录所有执行时间超过long_query_time秒的所有查询或不使用索引的查询。
* f、Innodb日志：innodb redo log

缺省情况下，所有日志创建于mysqld数据目录中。

可以通过刷新日志，来强制mysqld来关闭和重新打开日志文件（或者在某些情况下切换到一个新的日志）。

当你执行一个FLUSH LOGS语句或执行mysqladmin flush-logs或mysqladmin refresh时，则日志被老化。

对于存在MySQL复制的情形下，从复制服务器将维护更多日志文件，被称为接替日志。

### 2、二进制日志(Binary log)

**a、它包含的内容及作用如下：*

包含了所有更新了数据或者已经潜在更新了数据(比如没有匹配任何行的一个DELETE)

包含关于每个更新数据库(DML)的语句的执行时间信息

不包含没有修改任何数据的语句，如果需要启用该选项，需要开启通用日志功能

主要目的是尽可能的将数据库恢复到数据库故障点，因为二进制日志包含备份后进行的所有更新

用于在主复制服务器上记录所有将发送给从服务器的语句

启用该选项数据库性能降低1%，但保障数据库完整性，对于重要数据库值得以性能换完整。有些类似于Oracle开启归档模式。
  
**b、开启二进制日志的方法及属性**

使用--log-bin[=file_name]选项或在配置文件中指定log-bin启动时，mysqld写入包含所有更新数据的SQL命令的日志文件。

对于未给出file_name值， 默认名为-bin后面所跟的主机名。

在未指定绝对路径的情形下，缺省位置保存在数据目录下。

每个二进制日志名会添加一个数字扩展名用于日志老化，因此不支持自定义的扩展名，会被mysql数字扩展名动态替换。

若当前的日志大小达到max_binlog_size，则自动创建新的二进制日志。

对于大的事务，二进制日志会超过max_binlog_size设定的值。也即是事务仅仅写入一个二进制日志。

由是可知，二进制日志文件大小接近，其size不是完全相等，这点不同于oracle。

二进制日志文件会有一个对应二进制日志索引文件，该文件包含所有的二进制日志，其文件名与二进制日志相同，扩展名为.index

二进制索引文件通过--log-bin-index[=file_name]选项来指定

RESET MASTER语句将删除所有二进制日志文件，这将影响到从库。也可以用PURGE MASTER LOGS只删除部分二进制文件。

### 3、二进制日志相关演示

**a、启用二进制日志**

--当前环境

    root@localhost[(none)]> show variables like '%version%';
    +-------------------------+------------------------------+
    | Variable_name          | Value                        |
    +-------------------------+------------------------------+
    | innodb_version          | 5.5.39                      |
    | protocol_version        | 10                          |
    | slave_type_conversions  |                              |
    | version                | 5.5.39                      |
    | version_comment        | MySQL Community Server (GPL) |
    | version_compile_machine | x86_64                      |
    | version_compile_os      | Linux                        |
    +-------------------------+------------------------------+

    root@localhost[(none)]> show variables like '%log_bin%';
    +---------------------------------+-------+
    | Variable_name                  | Value |
    +---------------------------------+-------+
    | log_bin                        | OFF  | --该参数用于设定是否启用二进制日志
    | log_bin_trust_function_creators | OFF  |
    | sql_log_bin                    | ON    |
    +---------------------------------+-------+

--以下为binary log相关参数

    root@localhost[(none)]> show variables like '%binlog%';
    +-----------------------------------------+----------------------+
    | Variable_name                          | Value                |
    +-----------------------------------------+----------------------+
    | binlog_cache_size                      | 32768                |
    | binlog_direct_non_transactional_updates | OFF                  |
    | binlog_format                          | STATEMENT            |
    | binlog_stmt_cache_size                  | 32768                |
    | innodb_locks_unsafe_for_binlog          | OFF                  |
    | max_binlog_cache_size                  | 18446744073709547520 |
    | max_binlog_size                        | 1073741824          |
    | max_binlog_stmt_cache_size              | 18446744073709547520 |
    | sync_binlog                            | 0                    |
    +-----------------------------------------+----------------------+

--当前mysql服务器数据文件的缺省位置

    root@localhost[(none)]> show variables like '%datadir%';
    +---------------+-----------------+
    | Variable_name | Value          |
    +---------------+-----------------+
    | datadir      | /var/lib/mysql/ |
    +---------------+-----------------+

--停止mysql服务器

    SUSE11b:~ # service mysql stop
    Shutting down MySQL....                                              done

--编辑my.cnf来设定binary log日志位置(注，配置二进制日志路径及文件名后，系统变量log_bin被自动置为on)

    suse11b:~ # vi /etc/my.cnf
    suse11b:~ # grep -v ^# /etc/my.cnf
    [mysqld]
    log-error=/tmp/suse11b.err
    log_bin=/var/lib/mysql/binarylog/binlog
    suse11b:~ # mkdir -p /var/lib/mysql/binarylog
    suse11b:~ # chown -R mysql:mysql /var/lib/mysql/binarylog

    suse11b:~ # /etc/init.d/mysql start
    Starting MySQL..                                                      done
    suse11b:~ # ls -hltr /var/lib/mysql/binarylog/*
    -rw-rw---- 1 mysql mysql  39 Oct  3 13:41 /var/lib/mysql/binarylog/binlog.index  #索引文件
    -rw-rw---- 1 mysql mysql 107 Oct  3 13:41 /var/lib/mysql/binarylog/binlog.000001 #日志文件

**b、切换日志**

    suse11b:~ # mysql -uroot -pxxx
    root@localhost[(none)]> flush logs;
    Query OK, 0 rows affected (0.04 sec)

    root@localhost[(none)]> system ls -hltr /var/lib/mysql/binarylog/*
    -rw-rw---- 1 mysql mysql  78 Oct  3 13:43 /var/lib/mysql/binarylog/binlog.index
    -rw-rw---- 1 mysql mysql 107 Oct  3 13:43 /var/lib/mysql/binarylog/binlog.000002  #切换后产生了000002
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:43 /var/lib/mysql/binarylog/binlog.000001

    root@localhost[(none)]> system mysqladmin flush-logs    #使用mysqladmin命令行工具flush-logs方式切换日志
    root@localhost[(none)]> system ls -hltr /var/lib/mysql/binarylog/*
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:43 /var/lib/mysql/binarylog/binlog.000001
    -rw-rw---- 1 mysql mysql 117 Oct  3 13:45 /var/lib/mysql/binarylog/binlog.index
    -rw-rw---- 1 mysql mysql 107 Oct  3 13:45 /var/lib/mysql/binarylog/binlog.000003  #切换后产生了000003
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:45 /var/lib/mysql/binarylog/binlog.000002

    root@localhost[(none)]> system mysqladmin refresh      #使用mysqladmin命令行工具refresh方式切换日志
    root@localhost[(none)]> system ls -hltr /var/lib/mysql/binarylog/*
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:43 /var/lib/mysql/binarylog/binlog.000001
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:45 /var/lib/mysql/binarylog/binlog.000002
    -rw-rw---- 1 mysql mysql 156 Oct  3 13:46 /var/lib/mysql/binarylog/binlog.index
    -rw-rw---- 1 mysql mysql 107 Oct  3 13:46 /var/lib/mysql/binarylog/binlog.000004  #切换后产生了000004
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:46 /var/lib/mysql/binarylog/binlog.000003

**c、模拟产生二进制日志及查看内容**

    root@localhost[(none)]> create database tempdb;
    Query OK, 1 row affected (0.00 sec)

    root@localhost[(none)]> use tempdb
    Database changed
    root@localhost[tempdb]> create table tb1(id smallint,val varchar(10));
    Query OK, 0 rows affected (0.00 sec)

    root@localhost[tempdb]> insert into tb1 values(1,'jack');
    Query OK, 1 row affected (0.01 sec)

    root@localhost[tempdb]> system strings /var/lib/mysql/binarylog/binlog.000004
    bin?8.T
    5.5.39-log
    z=.T
    tempdb
    create database tempdb
    tempdb
    create table tb1(id smallint,val varchar(10))
    tempdb
    BEGIN
    tempdb
    insert into tb1 values(1,'jack')

    root@localhost[tempdb]> system more /var/lib/mysql/binarylog/binlog.index
    /var/lib/mysql/binarylog/binlog.000001
    /var/lib/mysql/binarylog/binlog.000002
    /var/lib/mysql/binarylog/binlog.000003
    /var/lib/mysql/binarylog/binlog.000004

--使用命令行工具mysqlbinlog直接提取二进制日志的内容

    root@localhost[tempdb]> system mysqlbinlog /var/lib/mysql/binarylog/binlog.000004
    /*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
    /*!40019 SET @@session.max_insert_delayed_threads=0*/;
    /*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
    DELIMITER /*!*/;
    # at 4
    #141003 13:46:39 server id 1  end_log_pos 107  Start: binlog v 4, server v 5.5.39-log created 141003 13:46:39
    # Warning: this binlog is either in use or was not closed properly.
    BINLOG '
    PzguVA8BAAAAZwAAAGsAAAABAAQANS41LjM5LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAAVAAEGggAAAAICAgCAA==
    '/*!*/;
    # at 107
    #141003 14:08:58 server id 1  end_log_pos 194  Query  thread_id=1    exec_time=0    error_code=0
    SET TIMESTAMP=1412316538/*!*/;
    SET @@session.pseudo_thread_id=1/*!*/;
    SET @@session.foreign_key_checks=1, @@session.sql_auto_is_null=0, @@session.unique_checks=1, @@session.autocommit=1/*!*/;
    SET @@session.sql_mode=0/*!*/;
    SET @@session.auto_increment_increment=1, @@session.auto_increment_offset=1/*!*/;
    /*!\C utf8 *//*!*/;
    SET @@session.character_set_client=33,@@session.collation_connection=33,@@session.collation_server=8/*!*/;
    SET @@session.lc_time_names=0/*!*/;
    SET @@session.collation_database=DEFAULT/*!*/;
    create database tempdb
    /*!*/;
    # at 194
    #141003 14:09:36 server id 1  end_log_pos 304  Query  thread_id=1    exec_time=0    error_code=0
    use `tempdb`/*!*/;
    SET TIMESTAMP=1412316576/*!*/;
    create table tb1(id smallint,val varchar(10))
    /*!*/;
    # at 304
    #141003 14:09:56 server id 1  end_log_pos 374  Query  thread_id=1    exec_time=0    error_code=0
    SET TIMESTAMP=1412316596/*!*/;
    BEGIN
    /*!*/;
    # at 374
    #141003 14:09:56 server id 1  end_log_pos 471  Query  thread_id=1    exec_time=0    error_code=0
    SET TIMESTAMP=1412316596/*!*/;
    insert into tb1 values(1,'jack')
    /*!*/;
    # at 471
    #141003 14:09:56 server id 1  end_log_pos 498  Xid = 25
    COMMIT/*!*/;
    DELIMITER ;
    # End of log file
    ROLLBACK /* added by mysqlbinlog */;
    /*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
    /*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;

--从以上的内容可以看出二进制日志记录了所有操作的DML语句及其开销，以及一些系统环境变量的信息。


**d、管理二进制日志**

--对于二进制日志，应尽可能保存在安全的位置，与数据分开存储  

--使用show binary logs获取二进制日志相关信息

    root@localhost[(none)]> help show binary logs;
    Name: 'SHOW BINARY LOGS'
    Description:
    Syntax:
    SHOW BINARY LOGS
    SHOW MASTER LOGS

    Lists the binary log files on the server. This statement is used as
    part of the procedure described in [HELP PURGE BINARY LOGS], that shows
    how to determine which logs can be purged.

    root@localhost[tempdb]> show binary logs;
    +---------------+-----------+
    | Log_name      | File_size |
    +---------------+-----------+
    | binlog.000001 |      147 |
    | binlog.000002 |      147 |
    | binlog.000003 |      147 |
    | binlog.000004 |      498 |
    +---------------+-----------+

show binlog events用于在二进制日志中显示事件。如果未指定'log_name'，则显示第一个二进制日志。

    root@localhost[(none)]> help show binlog events;  --获取帮助信息
    Name: 'SHOW BINLOG EVENTS'
    Description:
    Syntax:
    SHOW BINLOG EVENTS
      [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count]

    Shows the events in the binary log. If you do not specify 'log_name',
    the first binary log is displayed.

    root@localhost[(none)]> show binlog events;
    +---------------+-----+-------------+-----------+-------------+---------------------------------------+
    | Log_name      | Pos | Event_type  | Server_id | End_log_pos | Info                                  |
    +---------------+-----+-------------+-----------+-------------+---------------------------------------+
    | binlog.000001 |  4 | Format_desc |        1 |        107 | Server ver: 5.5.39-log, Binlog ver: 4 |
    | binlog.000001 | 107 | Rotate      |        1 |        147 | binlog.000002;pos=4                  |
    +---------------+-----+-------------+-----------+-------------+---------------------------------------+

    root@localhost[(none)]> show binlog events in 'binlog.000005';  --binlog.000005不存在，所以报错
    ERROR 1220 (HY000): Error when executing command SHOW BINLOG EVENTS: Could not find target log

--下面的这个查询中，前面执行的DML在这里均可以看到

    root@localhost[tempdb]> show binlog events in 'binlog.000004';
    +---------------+-----+-------------+-----------+-------------+-------------------------------------------------------------+
    | Log_name      | Pos | Event_type  | Server_id | End_log_pos | Info                                                        |
    +---------------+-----+-------------+-----------+-------------+-------------------------------------------------------------+
    | binlog.000004 |  4 | Format_desc |        1 |        107 | Server ver: 5.5.39-log, Binlog ver: 4                      |
    | binlog.000004 | 107 | Query      |        1 |        194 | create database tempdb                                      |
    | binlog.000004 | 194 | Query      |        1 |        304 | use `tempdb`; create table tb1(id smallint,val varchar(10)) |
    | binlog.000004 | 304 | Query      |        1 |        374 | BEGIN                                                      |
    | binlog.000004 | 374 | Query      |        1 |        471 | use `tempdb`; insert into tb1 values(1,'jack')              |
    | binlog.000004 | 471 | Xid        |        1 |        498 | COMMIT /* xid=25 */                                        |
    +---------------+-----+-------------+-----------+-------------+-------------------------------------------------------------+

    root@localhost[tempdb]> show binlog events in 'binlog.000004' from 374;
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+
    | Log_name      | Pos | Event_type | Server_id | End_log_pos | Info                                          |
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+
    | binlog.000004 | 374 | Query      |        1 |        471 | use `tempdb`; insert into tb1 values(1,'jack') |
    | binlog.000004 | 471 | Xid        |        1 |        498 | COMMIT /* xid=25 */                            |
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+

    root@localhost[tempdb]> show binlog events in 'binlog.000004' from 374 limit 1;
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+
    | Log_name      | Pos | Event_type | Server_id | End_log_pos | Info                                          |
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+
    | binlog.000004 | 374 | Query      |        1 |        471 | use `tempdb`; insert into tb1 values(1,'jack') |
    +---------------+-----+------------+-----------+-------------+------------------------------------------------+

**d、删除历史日志**

--使用purge手动删除指定日志

--使用expire-log-days删除失效日志，设置变量expire_logs_days，删除超出这个变量保留期之前的所有日志被删除

--自动日志删除通常发生在服务器启动以及日志flush

--reset master方式

    root@localhost[(none)]> help purge;
    Name: 'PURGE BINARY LOGS'
    Description:
    Syntax:
    PURGE { BINARY | MASTER } LOGS
        { TO 'log_name' | BEFORE datetime_expr }

    Examples:
    PURGE BINARY LOGS TO 'mysql-bin.010';
    PURGE BINARY LOGS BEFORE '2008-04-02 22:46:26';

    root@localhost[tempdb]> purge binary logs to 'binlog.000003';
    Query OK, 0 rows affected (0.12 sec)

    root@localhost[tempdb]> show binary logs;
    +---------------+-----------+
    | Log_name      | File_size |
    +---------------+-----------+
    | binlog.000003 |      147 |
    | binlog.000004 |      498 |
    +---------------+-----------+

    root@localhost[tempdb]> system ls -hltr /var/lib/mysql/binarylog/*
    -rw-rw---- 1 mysql mysql 147 Oct  3 13:46 /var/lib/mysql/binarylog/binlog.000003
    -rw-rw---- 1 mysql mysql 498 Oct  3 14:09 /var/lib/mysql/binarylog/binlog.000004
    -rw-rw---- 1 mysql mysql  78 Oct  3 14:23 /var/lib/mysql/binarylog/binlog.index

--使用before子句purge日志，binlog.000003被删除

    root@localhost[tempdb]> purge binary logs before '2014-10-03 14:09:56';
    Query OK, 0 rows affected (0.02 sec)

    root@localhost[tempdb]> show binary logs;
    +---------------+-----------+
    | Log_name      | File_size |
    +---------------+-----------+
    | binlog.000004 |      498 |
    +---------------+-----------+

--重置所有日志

--reset master将删除在索引文件中列出所有的日志文件并重置索引文件，最后生成一个新的binlog文件。

--该操作之前先备份binlog至其它位置以备以后需要。

    root@localhost[tempdb]> help reset master;
    Name: 'RESET MASTER'
    Description:
    Syntax:
    RESET MASTER

    Deletes all binary log files listed in the index file, resets the
    binary log index file to be empty, and creates a new binary log file.
    This statement is intended to be used only when the master is started
    for the first time.

    root@localhost[tempdb]> reset master;
    Query OK, 0 rows affected (0.13 sec)

    root@localhost[tempdb]> show binary logs;
    +---------------+-----------+
    | Log_name      | File_size |
    +---------------+-----------+
    | binlog.000001 |      107 |  --reset之后，从000001开始生成全新空日志
    +---------------+-----------+

--expire_log系统变量控制二进制日志自动删除的天数。默认值为0,表示“没有自动删除”。启动时和二进制日志循环时可能删除。

    root@localhost[tempdb]> show variables like 'expire_log%';
    +------------------+-------+
    | Variable_name    | Value |
    +------------------+-------+
    | expire_logs_days | 0    |
    +------------------+-------+

    root@localhost[tempdb]> set expire_logs_days=7;  --提示次系统变量为全局变量
    ERROR 1229 (HY000): Variable 'expire_logs_days' is a GLOBAL variable and should be set with SET GLOBAL
    root@localhost[tempdb]> set global expire_logs_days=7;  --设置
    Query OK, 0 rows affected (0.01 sec)

    root@localhost[tempdb]> select @@expire_logs_days;
    +--------------------+
    | @@expire_logs_days |
    +--------------------+
    |                  7 |
    +--------------------+
