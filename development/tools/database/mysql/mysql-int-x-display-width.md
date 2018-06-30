# mysql int(x) 显示宽度

### 解释：

MySQL中的 int(x)，x表示此列的显示宽度，x位；显示宽度经常会与zerofill一起使用，插入值不足x位，则左侧补零，超过x位，则正常显示。注意，显示宽度不会影响值得存储，仅仅影响外观。

int等存储值的范围始终如下表：

    Storage Requirements for Numeric Types
    Data Type	Storage Required
    TINYINT	1 byte
    SMALLINT	2 bytes
    MEDIUMINT	3 bytes
    INT, INTEGER	4 bytes
    BIGINT	8 bytes
    FLOAT(p)	4 bytes if 0 <= p <= 24, 8 bytes if 25 <= p <= 53
    FLOAT	4 bytes
    DOUBLE [PRECISION], REAL	8 bytes
    DECIMAL(M,D), NUMERIC(M,D)	Varies; see following discussion
    BIT(M)	approximately (M+7)/8 bytes


经常会遇见一种错误的观点，比如 int(2)，就认为最大存储的值是99，其实是不对的。

### 模拟显示宽度：

    create table user(id int(5) zerofill);  
    insert into user value(1);  
    select * from user;  

![](http://biang.io/biangpic/blog/782c811355b9f2a8eeeeae98c03ffc61.png)
