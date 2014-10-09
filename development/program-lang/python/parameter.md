python 函数参数的传递(参数带星号的说明)
=======================================

python中函数参数的传递是通过赋值来传递的。函数参数的使用又有俩个方面值得注意：

1.函数参数是如何定义的 

2.在调用函数的过程中参数是如何被解析

---

先看第一个问题，在python中函数参数的定义主要有四种方式：

### 1.F(arg1,arg2,...)

这 是最常见的定义方式，一个函数可以定义任意个参数，每个参数间用逗号分割，用这种方式定义的函数在调用的的时候也必须在函数名后的小括号里提供个数相等的 值（实际参数），而且顺序必须相同，也就是说在这种调用方式中，形参和实参的个数必须一致，而且必须一一对应，也就是说第一个形参对应这第一个实参。例 如：

	def a(x,y):
	    print x,y

调用该函数，a(1,2)则x取1，y取2，形参与实参相对应，如果a(1)或者a(1,2,3)则会报错。


### 2.F(arg1,arg2=value2,...)

这种方式就是第一种的改进版，提供了默认值

	def a(x,y=3):
	    print x,y

调用该函数，a(1,2)同样还是x取1，y取2，但是如果a(1)，则不会报错了，这个时候x还是1，y则为默认的3。上面这俩种方式，还可以更换参数位置，比如a(y=8,x=3)用这种形式也是可以的。


### 3.F(*arg1)

上面俩个方式是有多少个形参，就传进去多少个实参，但有时候会不确定有多少个参数，则此时第三种方式就比较有用，它以一个*加上形参名的方式来表示这个函数 的实参个数不定，可能为0个也可能为n个。注意一点是，不管有多少个，在函数内部都被存放在以形参名为标识符的tuple中。

	>>> def a(*x):
	if len(x)==0:
	print 'None'
	else:
	print x
	>>> a(1)
	(1,)        #存放在元组中
	>>> a()
	None
	>>> a(1,2,3)
	(1, 2, 3)
	>>> a(m=1,y=2,z=3)

	Traceback (most recent call last):
	File "<pyshell#16>", line 1, in -toplevel-
	a(m=1,y=2,z=3)
	TypeError: a() got an unexpected keyword argument 'm'


### 4.F(**arg1)

形参名前加俩个*表示，参数在函数内部将被存放在以形式名为标识符的dictionary中，这时调用函数的方法则需要采用arg1=value1,arg2=value2这样的形式。

	>>> def a(**x):
	if len(x)==0:
	print 'None'
	else:
	print x  
	>>> a()
	None
	>>> a(x=1,y=2)
	{'y': 2, 'x': 1}      #存放在字典中
	>>> a(1,2)            #这种调用则报错

	Traceback (most recent call last):
	File "<pyshell#25>", line 1, in -toplevel-
	a(1,2)
	TypeError: a() takes exactly 0 arguments (2 given)


---

上面介绍了四种定义方式，接下来看函数参数在调用过程中是怎么被解析的,其实只要记住上面这四种方法优先级依次降低，先1，后2，再3，最后4，也就是先把方式1中的arg解析，然后解析方式2中的arg=value，再解析方式3，即是把多出来的arg这种形式的实参组成个tuple传进去，最后把剩下的key=value这种形式的实参组成一个dictionary传给带俩个星号的形参，也就方式4。

	>>> def test(x,y=1,*a,**b):
	print x,y,a,b


	>>> test(1)
	1 1 () {}
	>>> test(1,2)
	1 2 () {}
	>>> test(1,2,3)
	1 2 (3,) {}
	>>> test(1,2,3,4)
	1 2 (3, 4) {}
	>>> test(x=1,y=2)
	1 2 () {}
	>>> test(1,a=2)
	1 1 () {'a': 2}
	>>> test(1,2,3,a=4)
	1 2 (3,) {'a': 4}
	>>> test(1,2,3,y=4)

	Traceback (most recent call last):
	File "<pyshell#52>", line 1, in -toplevel-
	test(1,2,3,y=4)
	TypeError: test() got multiple values for keyword argument 'y'
