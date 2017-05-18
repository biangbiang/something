# MySQL内置函数获取几天前的日期

如何采用MySQL内置函数获取指定时间之前的日期呢？

    SELECT　something　FROM　table_name　WHERE　DATE_SUB(CURDATE(),INTERVAL　30　DAY)　<=　date_col;  获取30天前的日期。

    select date_sub(now(),interval 1 year); 获取一年前的今天的日期

    select date_sub(now(),interval 1 week); 获取一周前的日期

    select date_sub(now(),interval 1 month); 获取一个月前的日期

### mysql与日期相关的函数：

    SELECT DATE(createTime) date型
    SELECT DATE_FORMAT(createTime, '%Y-%m-%d') varchar类型



## 下面的一大堆相关介绍，在网上复制过来的：

### mySQL中常用日期时间函数：

下面的查询选择了所有记录，其date_col的值是在最后30天以内：

    mysql> SELECT something FROM table
    WHERE TO_DAYS(NOW()) - TO_DAYS(date_col)<= 30;

##### DAYOFWEEK(date)

返回日期date的星期索引(1=星期天，2=星期一, ……7=星期六)。这些索引值对应于ODBC标准。

    mysql> select DAYOFWEEK('1998-02-03');
    -> 3

##### WEEKDAY(date)

返回date的星期索引(0=星期一，1=星期二, ……6= 星期天)。

    mysql> select WEEKDAY('1997-10-04 22:23:00');
    -> 5
    mysql> select WEEKDAY('1997-11-05');
    -> 2

##### DAYOFMONTH(date)

返回date的月份中日期，在1到31范围内。

    mysql> select DAYOFMONTH('1998-02-03');
    -> 3

##### DAYOFYEAR(date)

返回date在一年中的日数, 在1到366范围内。

    mysql> select DAYOFYEAR('1998-02-03');
    -> 34

##### MONTH(date)

返回date的月份，范围1到12。

    mysql> select MONTH('1998-02-03');
    -> 2

##### DAYNAME(date)

返回date的星期名字。

    mysql> select DAYNAME("1998-02-05");
    -> 'Thursday'

##### MONTHNAME(date)

返回date的月份名字。

    mysql> select MONTHNAME("1998-02-05");
    -> 'February'

##### QUARTER(date)

返回date一年中的季度，范围1到4。

    mysql> select QUARTER('98-04-01');
    -> 2

##### WEEK(date) / WEEK(date,first)

对于星期天是一周的第一天的地方，有一个单个参数，返回date的周数，范围在0到52。2个参数形式WEEK()允许你指定星期是否开始于星期天或星期一。如果第二个参数是0，星期从星期天开始，如果第二个参数是1，从星期一开始。

    mysql> select WEEK('1998-02-20');
    -> 7
    mysql> select WEEK('1998-02-20',0);
    -> 7
    mysql> select WEEK('1998-02-20',1);
    -> 8

##### YEAR(date)

返回date的年份，范围在1000到9999。

    mysql> select YEAR('98-02-03');
    -> 1998

##### HOUR(time)

返回time的小时，范围是0到23。

    mysql> select HOUR('10:05:03');
    -> 10

##### MINUTE(time)

返回time的分钟，范围是0到59。

    mysql> select MINUTE('98-02-03 10:05:03');
    -> 5

##### SECOND(time)

回来time的秒数，范围是0到59。

    mysql> select SECOND('10:05:03');
    -> 3

##### PERIOD\_ADD(P,N)

增加N个月到阶段P（以格式YYMM或YYYYMM)。以格式YYYYMM返回值。注意阶段参数P不是日期值。

    mysql> select PERIOD_ADD(9801,2);
    -> 199803

##### PERIOD\_DIFF(P1,P2)

返回在时期P1和P2之间月数，P1和P2应该以格式YYMM或YYYYMM。注意，时期参数P1和P2不是日期值。

    mysql> select PERIOD_DIFF(9802,199703);
    -> 11

##### DATE\_ADD(date,INTERVAL expr type)
　
##### DATE\_SUB(date,INTERVAL expr type)
　
##### ADDDATE(date,INTERVAL expr type)
　
##### SUBDATE(date,INTERVAL expr type)

这些功能执行日期运算。对于MySQL 3.22，他们是新的。ADDDATE()和SUBDATE()是DATE\_ADD()和DATE\_SUB()的同义词。

在MySQL 3.23中，你可以使用+和-而不是DATE\_ADD()和DATE\_SUB()。（见例子）date是一个指定开始日期的DATETIME或DATE值，expr是指定加到开始日期或从开始日期减去的间隔值一个表达式，expr是一个字符串；它可以以 一个“-”开始表示负间隔。type是一个关键词，指明表达式应该如何被解释。EXTRACT(type FROM date)函数从日期中返回“type”间隔。下表显示了type和expr参数怎样被关联： 

    type值 含义 期望的expr格式
    SECOND 秒 SECONDS
    MINUTE 分钟 MINUTES
    HOUR 时间 HOURS
    DAY 天 DAYS
    MONTH 月 MONTHS
    YEAR 年 YEARS
    MINUTE_SECOND 分钟和秒 "MINUTES:SECONDS"
    HOUR_MINUTE 小时和分钟 "HOURS:MINUTES"
    DAY_HOUR 天和小时 "DAYS HOURS"
    YEAR_MONTH 年和月 "YEARS-MONTHS"
    HOUR_SECOND 小时, 分钟， "HOURS:MINUTES:SECONDS"
    DAY_MINUTE 天, 小时, 分钟 "DAYS HOURS:MINUTES"
    DAY_SECOND 天, 小时, 分钟, 秒 "DAYS HOURS:MINUTES:SECONDS"

MySQL在expr格式中允许任何标点分隔符。表示显示的是建议的分隔符。如果date参数是一个DATE值并且你的计算仅仅包含YEAR、MONTH和DAY部分(即，没有时间部分)，结果是一个DATE值。否则结果是一个DATETIME值。

    mysql> SELECT "1997-12-31 23:59:59" + INTERVAL 1 SECOND;
    -> 1998-01-01 00:00:00

    mysql> SELECT INTERVAL 1 DAY + "1997-12-31";
    -> 1998-01-01

    mysql> SELECT "1998-01-01" - INTERVAL 1 SECOND;
    -> 1997-12-31 23:59:59

    mysql> SELECT DATE_ADD("1997-12-31 23:59:59",
    INTERVAL 1 SECOND);
    -> 1998-01-01 00:00:00

    mysql> SELECT DATE_ADD("1997-12-31 23:59:59",
    INTERVAL 1 DAY);
    -> 1998-01-01 23:59:59

    mysql> SELECT DATE_ADD("1997-12-31 23:59:59",
    INTERVAL "1:1" MINUTE_SECOND);
    -> 1998-01-01 00:01:00

    mysql> SELECT DATE_SUB("1998-01-01 00:00:00",
    INTERVAL "1 1:1:1" DAY_SECOND);
    -> 1997-12-30 22:58:59

    mysql> SELECT DATE_ADD("1998-01-01 00:00:00",
    INTERVAL "-1 10" DAY_HOUR);
    -> 1997-12-30 14:00:00

    mysql> SELECT DATE_SUB("1998-01-02", INTERVAL 31 DAY);
    -> 1997-12-02

    mysql> SELECT EXTRACT(YEAR FROM "1999-07-02");
    -> 1999

    mysql> SELECT EXTRACT(YEAR_MONTH FROM "1999-07-02 01:02:03");
    -> 199907

    mysql> SELECT EXTRACT(DAY_MINUTE FROM "1999-07-02 01:02:03");
    -> 20102

如果你指定太短的间隔值(不包括type关键词期望的间隔部分)，MySQL假设你省掉了间隔值的最左面部分。例如，如果你指定一个type是 DAY_SECOND，值expr被希望有天、小时、分钟和秒部分。如果你象"1:10"这样指定值，MySQL假设日子和小时部分是丢失的并且值代表分 钟和秒。换句话说，"1:10" DAY_SECOND以它等价于"1:10" MINUTE_SECOND 的方式解释，这对那MySQL解释TIME值表示经过的时间而非作为一天的时间的方式有二义性。如果你使用确实不正确的日期，结果是NULL。如果你增加 MONTH、YEAR_MONTH或YEAR并且结果日期大于新月份的最大值天数，日子在新月用最大的天调整。

    mysql> select DATE_ADD('1998-01-30', Interval 1 month);
    -> 1998-02-28

注意，从前面的例子中词INTERVAL和type关键词不是区分大小写的。

TO\_DAYS(date) 给出一个日期date，返回一个天数(从0年的天数)。

    mysql> select TO_DAYS(950501);
    -> 728779
    mysql> select TO_DAYS('1997-10-07');
    -> 729669

TO\_DAYS()不打算用于使用格列高里历(1582)出现前的值。

    FROM_DAYS(N)

给出一个天数N，返回一个DATE值。

    mysql> select FROM_DAYS(729669);
    -> '1997-10-07'

TO\_DAYS()不打算用于使用格列高里历(1582)出现前的值。

##### DATE\_FORMAT(date,format)

根据format字符串格式化date值。下列修饰符可以被用在format字符串中： 

    %M 月名字(January……December)
    %W 星期名字(Sunday……Saturday)
    %D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）
    %Y 年, 数字, 4 位
    %y 年, 数字, 2 位
    %a 缩写的星期名字(Sun……Sat)
    %d 月份中的天数, 数字(00……31)
    %e 月份中的天数, 数字(0……31)
    %m 月, 数字(01……12)
    %c 月, 数字(1……12)
    %b 缩写的月份名字(Jan……Dec)
    %j 一年中的天数(001……366)
    %H 小时(00……23)
    %k 小时(0……23)
    %h 小时(01……12)
    %I 小时(01……12)
    %l 小时(1……12)
    %i 分钟, 数字(00……59)
    %r 时间,12 小时(hh:mm:ss [AP]M)
    %T 时间,24 小时(hh:mm:ss)
    %S 秒(00……59)
    %s 秒(00……59)
    %p AM或PM
    %w 一个星期中的天数(0=Sunday ……6=Saturday ）
    %U 星期(0……52), 这里星期天是星期的第一天
    %u 星期(0……52), 这里星期一是星期的第一天
    %% 一个文字“%”。

所有的其他字符不做解释被复制到结果中。

    mysql> select DATE_FORMAT('1997-10-04 22:23:00', '%W %M %Y');
    -> 'Saturday October 1997'

    mysql> select DATE_FORMAT('1997-10-04 22:23:00', '%H:%i:%s');
    -> '22:23:00'

    mysql> select DATE_FORMAT('1997-10-04 22:23:00',
    '%D %y %a %d %m %b %j');
    -> '4th 97 Sat 04 10 Oct 277'

    mysql> select DATE_FORMAT('1997-10-04 22:23:00',
    '%H %k %I %r %T %S %w');
    -> '22 22 10 10:23:00 PM 22:23:00 00 6'

MySQL3.23中，在格式修饰符字符前需要%。在MySQL更早的版本中，%是可选的。

##### TIME\_FORMAT(time,format)

这象上面的DATE\_FORMAT()函数一样使用，但是format字符串只能包含处理小时、分钟和秒的那些格式修饰符。

其他修饰符产生一个NULL值或0。

##### CURDATE() 　

CURRENT\_DATE 以'YYYY-MM-DD'或YYYYMMDD格式返回今天日期值，取决于函数是在一个字符串还是数字上下文被使用。

    mysql> select CURDATE();
    -> '1997-12-15'
    mysql> select CURDATE() + 0;
    -> 19971215

##### CURTIME() 　

CURRENT\_TIME 以'HH:MM:SS'或HHMMSS格式返回当前时间值，取决于函数是在一个字符串还是在数字的上下文被使用。

    mysql> select CURTIME();
    -> '23:50:26'
    mysql> select CURTIME() + 0;
    -> 235026

NOW()
　
SYSDATE()
　
CURRENT\_TIMESTAMP 以'YYYY-MM-DD HH:MM:SS'或YYYYMMDDHHMMSS格式返回当前的日期和时间，取决于函数是在一个字符串还是在数字的上下文被使用。

    mysql> select NOW();
    -> '1997-12-15 23:50:26'
    mysql> select NOW() + 0;
    -> 19971215235026

##### UNIX\_TIMESTAMP() / UNIX\_TIMESTAMP(date)

如果没有参数调用，返回一个Unix时间戳记(从'1970-01-01 00:00:00'GMT开始的秒数)。如果UNIX_TIMESTAMP()用一个date参数被调用，它返回从'1970-01-01 00:00:00' GMT开始的秒数值。date可以是一个DATE字符串、一个DATETIME 字符串、一个TIMESTAMP或以YYMMDD或YYYYMMDD格式的本地时间的一个数字。

    mysql> select UNIX_TIMESTAMP();
    -> 882226357
    mysql> select UNIX_TIMESTAMP('1997-10-04 22:23:00');
    -> 875996580

当UNIX\_TIMESTAMP被用于一个TIMESTAMP列，函数将直接接受值，没有隐含的“string-to-unix-timestamp”变换。

##### FROM\_UNIXTIME(unix\_timestamp)

以'YYYY-MM-DD HH:MM:SS'或YYYYMMDDHHMMSS格式返回unix_timestamp参数所表示的值，取决于函数是在一个字符串还是或数字上下文中被使用。

    mysql> select FROM_UNIXTIME(875996580);
    -> '1997-10-04 22:23:00'
    mysql> select FROM_UNIXTIME(875996580) + 0;
    -> 19971004222300

##### FROM\_UNIXTIME(unix\_timestamp,format)

返回表示 Unix 时间标记的一个字符串，根据format字符串格式化。format可以包含与DATE_FORMAT()函数列出的条目同样的修饰符。

    mysql> select FROM_UNIXTIME(UNIX_TIMESTAMP(),
    '%Y %D %M %h:%i:%s %x');
    -> '1997 23rd December 03:43:30 x'

##### SEC\_TO\_TIME(seconds)

返回seconds参数，变换成小时、分钟和秒，值以'HH:MM:SS'或HHMMSS格式化，取决于函数是在一个字符串还是在数字上下文中被使用。

    mysql> select SEC_TO_TIME(2378);
    -> '00:39:38'
    mysql> select SEC_TO_TIME(2378) + 0;
    -> 3938

##### TIME\_TO\_SEC(time)

返回time参数，转换成秒。

    mysql> select TIME_TO_SEC('22:23:00');
    -> 80580
    mysql> select TIME_TO_SEC('00:39:38');
    -> 2378

常用时间日期的获取：

添加字段：

    alter table `user_movement_log`
    Add column GatewayId int not null default 0 AFTER `Regionid` (在哪个字段后面添加)

删除字段：

    alter table `user_movement_log` drop column Gatewayid

调整字段顺序：

    ALTER TABLE `user_movement_log` CHANGE `GatewayId` `GatewayId` int not null default 0 AFTER RegionID

通常我们会有一些时间的转换需求，比如要统计某个时间段的收入，比如要截取某个时间的年份，比如要根据某个日期推算出是星期几等个，这些都可以通过Mysql自带的时间函数很容易实现。因为我对Mysql的函数还不熟，而通常又会调用这些时间函数，这边稍加总结以便查询。

–返回当前时间

    mysql> select now(),date(now()),sysdate();
    +---------------------+-------------+---------------------+
    | now() | date(now()) | sysdate() |
    +---------------------+-------------+---------------------+
    | 2008-12-02 10:11:36 | 2008-12-02 | 2008-12-02 10:11:36 |
    +---------------------+-------------+---------------------+
    1 row in set (0.00 sec)
    mysql> select curdate(),curdate()+0,curtime(),curtime()+0;
    +---------------------+-------------+---------------------+
    | curdate() | curdate()+0 | curtime() | curtime()+0 |
    +---------------------+-------------+---------------------+
    | 2008-12-02 | 20081202 | 10:00:33 | 100033.000000 |
    +---------------------+-------------+---------------------+

–返回日期当月最后一天

    mysql> select last_day('2008-12-02');
    +------------------------+
    | last_day('2008-12-02') |
    +------------------------+
    | 2008-12-31 |
    +------------------------+
    1 row in set (0.00 sec)

–返回日期的星期几

    mysql> select dayname('2008-12-02'),dayofweek('2008-12-02');
    +-----------------------+-------------------------+
    | dayname('2008-12-02') | dayofweek('2008-12-02') |
    +-----------------------+-------------------------+
    | tuesday | 3 |
    +-----------------------+-------------------------+
    1 row in set (0.00 sec)

–返回日期的年，月，日

    mysql> select month('2008-12-02'),year('2008-12-02'),day('2008-12-02');
    +---------------------+--------------------+-------------------+
    | month('2008-12-02') | year('2008-12-02') | day('2008-12-02') |
    +---------------------+--------------------+-------------------+
    | 12 | 2008 | 2 |
    +---------------------+--------------------+-------------------+
    1 row in set (0.00 sec)

–返回日期的小时，分，秒

    mysql> select hour('10:05:03'),minute('10:05:03'),second('10:05:03');
    +------------------+--------------------+--------------------+
    | hour('10:05:03') | minute('10:05:03') | second('10:05:03') |
    +------------------+--------------------+--------------------+
    | 10 | 5 | 3 |
    +------------------+--------------------+--------------------+
    1 row in set (0.00 sec)

1.subdate(d,t)：起始时间加上一段时间（year,month,day…）

    mysql> select date_add('1998-01-02', interval 31 day),adddate('1998-01-02', 31);
    +-----------------------------------------+---------------------------+
    | date_add('1998-01-02', interval 31 day) | adddate('1998-01-02', 31) |
    +-----------------------------------------+---------------------------+
    | 1998-02-02 | 1998-02-02 |
    +-----------------------------------------+---------------------------+
    1 row in set (0.00 sec)
    mysql> select date_add('1998-01-02',interval 2 year);
    +-----------------------------------------------------+
    | date_add('1998-01-02', interval 2 year)
    +-----------------------------------------------------+
    | 2000-01-02
    +-----------------------------------------------------+
    1 row in set (0.00 sec)
    mysql> select date_add('1998-01-02', interval 2 hour);
    +-----------------------------------------------------+
    | date_add('1998-01-02', interval 2 hour)
    +-----------------------------------------------------+
    | 1998-01-02 02:00:00
    +-----------------------------------------------------+
    1 row in set (0.00 sec)

2.subdate(d,t)：起始时间减去一段时间

    mysql> select subdate('1998-01-02', interval 31 day),subdate('1998-01-02', 31);
    +----------------------------------------+---------------------------+
    | subdate('1998-01-02', interval 31 day) | subdate('1998-01-02', 31) |
    +----------------------------------------+---------------------------+
    | 1997-12-02 | 1997-12-02 |
    +----------------------------------------+---------------------------+
    1 row in set (0.00 sec)

3.addtime(d,t):起始时间d加入时间t

    mysql> select addtime('1997-12-31 23:59:50','00:00:05'), addtime('23:59:50','00:00:05') ;
    +-------------------------------------------+--------------------------------+
    | addtime('1997-12-31 23:59:50','00:00:05') | addtime('23:59:50','00:00:05') |
    +-------------------------------------------+--------------------------------+
    | 1997-12-31 23:59:55 | 23:59:55 |
    +-------------------------------------------+--------------------------------+
    1 row in set (0.00 sec)

4.subtime(d,t):起始时间d减去时间t

    mysql> select subtime('1997-12-31 23:59:50','00:00:05'), subtime('23:59:50','00:00:05');
    +-------------------------------------------+--------------------------------+
    | subtime('1997-12-31 23:59:50','00:00:05') | subtime('23:59:50','00:00:05') |
    +-------------------------------------------+--------------------------------+
    | 1997-12-31 23:59:45 | 23:59:45 |
    +-------------------------------------------+--------------------------------+
    1 row in set (0.00 sec)

5.datediff(d1,d2):返回起始时间d1和结束时间d2之间的天数

    mysql> select datediff('1997-12-31 23:59:59','1997-12-30');
    +----------------------------------------------+
    | datediff('1997-12-31 23:59:59','1997-12-30') |
    +----------------------------------------------+
    | 1 |
    +----------------------------------------------+
    1 row in set (0.00 sec)

6.date\_format(date,format):根据format字符串显示date值的格式

    mysql> select date_format('2008-12-02 22:23:00', '%y %m %m %h:%i:%s');
    +---------------------------------------------------------+
    | date_format('2008-12-02 22:23:00', '%y %m %m %h:%i:%s') |
    +---------------------------------------------------------+
    |             2008 12 12 22      :23:00 |
    +---------------------------------------------------------+
    1 row in set (0.00 sec)

7.str\_to\_date(str,format) 字符串转化为时间

    mysql> select str_to_date('04/31/2004', '%m/%d/%y %h:%i:s');
    +-----------------------------------------------+
    | str_to_date('04/31/2004', '%m/%d/%y %h:%i:s') |
    +-----------------------------------------------+
    | 2004-04-31 00:00:00 |
    +-----------------------------------------------+
    1 row in set (0.00 sec)

8.timestamp(expr) , timestamp(expr,expr2) :

对于一个单参数,该函数将日期或日期时间表达式 expr 作为日期时间值返回.对于两个参数, 它将时间表达式 expr2添加到日期或日期时间表达式 expr 中，将theresult作为日期时间值返回

    mysql> select timestamp('2003-12-31'), timestamp('2003-12-31 12:00:00','12:00:00');
    +-------------------------+---------------------------------------------+
    | timestamp('2003-12-31') | timestamp('2003-12-31 12:00:00','12:00:00') |
    +-------------------------+---------------------------------------------+
    | 2003-12-31 00:00:00 | 2004-01-01 00:00:00 |
    +-------------------------+---------------------------------------------+
    1 row in set (0.00 sec)

9.取当天0点0分，下一天0点0分

    mysql> select timestamp(date(sysdate())),timestamp(adddate(date(sysdate()),1));
    +----------------------------+---------------------------------------+
    | timestamp(date(sysdate())) | timestamp(adddate(date(sysdate()),1)) |
    +----------------------------+---------------------------------------+
    | 2008-12-02 00:00:00 | 2008-12-03 00:00:00 |
    +----------------------------+---------------------------------------+
    1 row in set (0.00 sec)
