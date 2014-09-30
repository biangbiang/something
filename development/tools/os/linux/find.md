find用法心得
===========

通过几个比较典型的find用法来学习find命令

### `find . -type f -exec ls -l {} \;`

从当前文件夹中查找普通文件，并且对每个匹配的文件执行“ls -l {}”操作；

-type表示按照文件类型来查找文件：

* b - 块设备文件。
* d - 目录。
* c - 字符设备文件。
* p - 管道文件。
* l - 符号链接文件。
* f - 普通文件。

### `find logs -type f -mtime +5 -exec rm {} \;`

从logs文件夹中查找最后修改日期在5天前的文件，并且删除它们；

* -mtime：Modify Time
* -atime: Access Time
* -ctime: Create Time

### `find /etc -name "passwd*" -exec grep "sam" {} \;`

从/etc文件夹中查找文件名以“passwd”开头的所有文件中，是否有“sam”字符串；

-name 后面的表达式不是正则表达式，而是通配符，“*.txt”,"*[lL]inux*"等等。

## 主要选项：

* -name：按照文件名查找文件。
* -perm：按照文件权限来查找文件。
* -prune：使用这一选项可以使f i n d命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被f i n d命令忽略。
* -user： 按照文件属主来查找文件。
* -group：按照文件所属的组来查找文件。
* -mtime -n +n：按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内，+ n表示文件更改时间距现在n天以前。F i n d命令还有- a t i m e和- c t i m e选项，但它们都和- m t i m e选项。
* -nogroup：查找无有效所属组的文件，即该文件所属的组在/ e t c / g r o u p s中不存在。
* -nouser：查找无有效属主的文件，即该文件的属主在/ e t c / p a s s w d中不存在。
* -newer file1 ! file2：查找更改时间比文件f i l e 1新但比文件f i l e 2旧的文件。
* -type 查找某一类型的文件
* -size 按文件大小查找，+100c，表示大于100字节，-10，表示小于10块（1块为512字节）

每个具体的选项，只好等到实际工作需要的时候再练习吧。
