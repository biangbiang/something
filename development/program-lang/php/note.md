php学习笔记
=========

### array_values()

返回一个包含给定数组中所有键值的数组，但不保留键名

---

### !!

两个感叹号可以将其它类型的变量直接转换为布尔型. 不过在PHP中这样做只是为了便于阅读, 通常并没有什么实际作用, 因为PHP本身就会进行隐式转换.

### 面向对象访问修饰符

* public（公共的、默认）在PHP5中如果类没有指定成员的访问修饰符，默认就是public的访问权限。
* protected（受保护的）被声明为protected的成员，只允许该类的子类进行访问。
* private（私有的 ） 被定义为private的成员，对于类内部所有成员都可见，没有访问限制。对类外部不允许访问。

### isset和empty

当要 判断一个变量是否已经声明的时候 可以使用 isset 函数

当要 判断一个变量是否已经赋予数据且不为空 可以用 empty 函数

当要 判断 一个变量 存在且不为空 先isset 函数 再用 empty 函数

isset返回值:

* 若变量不存在则返回 FALSE
* 若变量存在且其值为NULL，也返回 FALSE
* 若变量存在且值不为NULL，则返回 TURE

empty返回值:

* 若变量不存在则返回 TRUE
* 若变量存在且其值为""、0、"0"、NULL、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 TURE
* 若变量存在且值不为""、0、"0"、NULL、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 FALSE

### curl post

CURLOPT\_POSTFIELDS

全部数据使用HTTP协议中的"POST"操作来发送。要发送文件，在文件名前面加上@前缀并使用完整路径。这个参数可以通过urlencoded后的字符串类似'para1=val1&para2=val2&...'或使用一个以字段名为键值，字段数据为值的数组。如果value是一个数组，Content-Type头将会被设置成multipart/form-data。

### PHP 大小写转换的函数

几个常用的PHP大小写转换方法，记录一下：

1    strtolower()       //将字符串转换为小写形式

2    strtoupper()     //将字符串转换为大写形式

3    ucfirst()           //将字符串的第一个字符转换为大写形式

4    ucwords()       //将字符串中每一个单词的首字母转换为大写形式
