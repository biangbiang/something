Python 匿名函数
===============

### 1）匿名函数lambda

在Python中，可以使用lambda关键字定义匿名函数，并将此函数作为参数传入其他函数中。

lambda argument1, argument2, ... argumentN : expression using arguments.

lambad 参数1，参数2 ......参数N：表达式

例如：

	>>> add = lambda x, y : x + y        //x，y是参数，x+y是表达式
	>>> add(1,2)
	>>> 3

### 2）map（）

映射函数，可以按照参数中指定的函数序列进行映射，生成新的序列。

例如：

	>>> map(lambda x:x*2, range(5))
	>>> [0, 2, 4, 6, 8] 

### 3）reduce（）

reduce操作通过指定的函数对序列进行迭代运算。

例如：

	>>> reduce(lambda x, y : x * (y+1), range(9), 1)
	>>> 362880
	>>> add = lambda x, y: x+y
	>>> reduce(add, range(5)
	>>> 10

### 4）filter()

过滤器用户从一个序列中取出满足条件的子序列。

例如：

	>>> filter (lambda x: x%2 == 0, range(10))
	>>> [0, 2, 4, 6,8]
