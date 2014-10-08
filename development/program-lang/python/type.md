判断数据类型
============

type(x)可以看到变量目前的类型

---

判断类型可以用两种方法

1:  

	import types
	type(x) is types.IntType  # 判断是否int 类型
	type(x) is types.StringType #是否string类型
	... ...

2: 

	type(x) == type(1)  # 是否 int 类型
	type(x) in (type(u'') , type('')): # 是否string, unicode类型
	... ...

