# mysql中tinyint(1)与tinyint(2)的区别

tinyint 型的字段如果设置为UNSIGNED类型，只能存储从0到255的整数,不能用来储存负数。

tinyint 型的字段如果不设置UNSIGNED类型,存储-128到127的整数。 

1个tinyint型数据只占用一个字节;一个INT型数据占用四个字节。

这看起来似乎差别不大，但是在比较大的表中，字节数的增长是很快的。

tinyint(1)与tinyint(2)的区别可以从下面看出来

    CREATE TABLE `test` (                                    
              `id` int(11) NOT NULL AUTO_INCREMENT,                  
              `str` varchar(255) NOT NULL,                                       
              `state` tinyint(1) unsigned zerofill DEFAULT NULL,     
              `state2` tinyint(2) unsigned zerofill DEFAULT NULL,    
              `state3` tinyint(3) unsigned zerofill DEFAULT NULL,    
              `state4` tinyint(4) unsigned zerofill DEFAULT NULL,    
              PRIMARY KEY (`id`)                                     
            ) ENGINE=MyISAM AUTO_INCREMENT=6 DEFAULT CHARSET=utf8    
   
    insert into test (str,state,state2,state3,state4) values('csdn',4,4,4,4);  
    select * from test;  

结果：

    id  str    state    state2  state3  state4 
    1   csdn   4        04      004     0004 
