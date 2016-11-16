# mysql 从一个表中查数据，插入另一个表

不管是在网站开发还是在应用程序开发中，我们经常会碰到需要将MySQL或MS SQLServer某个表的数据批量导入到另一个表的情况，甚至有时还需要指定导入字段。
 
本文就将以MySQL数据库为例，介绍如何通过SQL命令行将某个表的所有数据或指定字段的数据，导入到目标表 中。此方法对于SQLServer数据库，也就是T-SQL来说，同样适用 。

类别一、 如果两张张表（导出表和目标表）的字段一致，并且希望插入全部数据，可以用这种方法：

    INSERT INTO  目标表  SELECT  * FROM  来源表 ;

例如，要将 articles 表插入到 newArticles 表中，则可以通过如下SQL语句实现：

    INSERT INTO  newArticles  SELECT  * FROM  articles ;

类别二、 如果只希望导入指定字段，可以用这种方法：

    INSERT INTO  目标表 (字段1, 字段2, ...)  SELECT   字段1, 字段2, ...   FROM  来源表 ;

请注意以上两表的字段必须一致，否则会出现数据转换错误。
 
    INSERT INTO TPersonnelChange(
        UserId,
        DepId,
        SubDepId,
        PostionType,
        AuthorityId,
        ChangeDateS,
        InsertDate,
        UpdateDate,
        SakuseiSyaId
    )SELECT
        UserId,
        DepId,
        SubDepId,
        PostionType,
        AuthorityId,
        DATE_FORMAT(EmployDate, '%Y%m%d'),
        NOW(),
        NOW(),
        1
    FROM
        TUserMst
    WHERE
        `Status` = 0
    AND QuitFlg = 0
    AND UserId > 2
