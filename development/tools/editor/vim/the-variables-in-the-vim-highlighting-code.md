# vim高亮代码中的变量

`:match ErrorMsg /name/`  在代码中用红色标记出name这个变量

执行`:hi`命令，会输出不同的高亮名称，其中ErrorMag就是一种高亮名称，可以替换为左侧的其他值

还可以继续高亮其他颜色

`:2match WildMenu /name2/`

`:3match  DiffAdd /name3/`
