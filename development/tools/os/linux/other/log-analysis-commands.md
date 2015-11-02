Linux日志分析常用命令
====================

### 1.查看文件内容

`cat`

    -n 显示行号

### 2.分页显示

`more`

    Enter 显示下一行

    空格 显示下一页

    F 显示下一屏

    B 显示上一屏

`less`

    /get 查询"get"字符串并高亮显示

### 3.显示文件尾

`tail`

    -f 不退出持续显示

    -n 显示文件最后n行

### 4.显示头文件

`head`

    -n 显示文件开始n行

### 5.内容排序

`sort`

    -n 按照数字排序

    -r 按照逆序排序

    -k 表示排序列

    -t 指定分隔符

### 6.字符统计

`wc`

    -l 统计文件中行数

    -c 统计文件字节数

    -L 查看最长行长度

    -w 查看文件包含多少个单词

### 7.查看重复出现的行

`uniq`

    -c 查看该行内容出现的次数

    -u 只显示出现一次的行

    -d 只显示重复出现的行

### 8.字符串查找

`grep`

### 9.文件查找

`find`

`which`

`whereis`

### 10.表达式求值

`expr`

### 11.归档文件

`tar`

`zip`

`unzip`

### 12.URL访问工具

`curl`

`wget`

### 13. 查看请求访问量

页面访问排名前十的IP

    cat access.log | cut -f1 -d " " | sort | uniq -c | sort -k 1 -r | head -10

页面访问排名前十的URL

    cat access.log | cut -f4 -d " " | sort | uniq -c | sort -k 1 -r | head -10

查看最耗时的页面

    cat access.log | sort -k 2 -n -r | head 10

### 14.大杀器

`sed`

    sed 's/xxx/hello' access.log 将 xxx 替换成 hello 输出（s是文本替换命令）

    sed -n '2,6p' access.log 只输出第第2到第6之间的行（-n表示输出指定的行）

    sed '/qq/d' access.log 删除包含qq的行（d是文本删除命令）

    sed '=' access.log 显示文件行号

    sed -e 'i\head' access.log 在每行的前面插入head字符串（i在行首插入命令）

    sed -e 'a\end' access.log 在每行的末尾追加end字符串（i在行尾追加命令）

    sed -e '/google/c\hello' access.log 查找google匹配的行，用hello替换（c是对行文本替换命令）

`awk`
