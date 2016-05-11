MySQL报 Waiting for table level lock”错误
=========================================

Why I have so many queries when I using ‘show processlist ‘

![](http://biangbiangpic.b0.upaiyun.com/blog/813c4f871febfbf160071da9550609e8.jpg)

And my CPU is 600+% used

![](http://biangbiangpic.b0.upaiyun.com/blog/1d5846740595a2e770106b38606ab878.jpg)

Is there anything i can do to improve mysql performance? Thanks

### 暂时解决方案 

    SELECT
     a.*,CONCAT("kill " ,a.id,";") 
    FROM
     information_schema.`PROCESSLIST` a
    WHERE
     a.STATE = 'Waiting for table level lock'

### kill死进程

    kill  ${进程id}

### 过渡解决方案

    Yes, you can change your storage engine from MyISAM to InnoDB - MyISAM only knows table level locking (when it writes to a record, it blocks the whole table), while InnoDB knows row level locking (it locks just the row you are writing to)

### 终极解决方案
 
做数据库读写分离，针对读和写 设置不同的数据库引擎
