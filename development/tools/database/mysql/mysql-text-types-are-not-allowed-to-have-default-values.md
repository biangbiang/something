# mysql text类型不允许有默认值

mysql text类型不允许有默认值

mysql error 1101 text类型不允许有默认值

根据 mysql5.0以上版本 strict mode (`STRICT_TRANS_TABLES`) 的限制：

不支持对not null字段插入null值

不支持对自增长字段插入''值，可插入null值

不支持 text 字段有默认值

在my.ini中将 `STRICT_TRANS_TABLES` 去掉即可。

但是这个比较危险的是自增字段也可以插入null值！而自增字段一般都是主键，聚集索引，真的存在null值就完蛋了。 
