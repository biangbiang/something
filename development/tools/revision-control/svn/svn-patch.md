svn生成patch和打（导入）patch文件的方法
=======================================

### 生成patch文件:

    svn diff > patchFile // 整个工程的变动生成patch
    或svn diff file > patchFile // 某个文件单独变动的patch
 
### svn回滚：

    svn revert FILE // 单个文件回滚
    svn revert DIR --depth=infinity // 整个目录进行递归回滚
 
### 打patch:

    patch -p0 < test.patch // -p0 选项要从当前目录查找目的文件（夹）

    patch -p1 < test.patch // -p1 选项要从当前目录查找目的文件，不包含patch中的最上级目录（夹）

例如两个版本以a,b开头，而a,b并不是真正有效地代码路径，则这时候需要使用"-p1"参数。

    a/src/...
    b/src/...
