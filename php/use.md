PHP 中的use关键字
=================

今天看osCommerce源码框架时index.php文件中用到了use这个关键字，

	use osCommerce\OM\Core\Autoloader;
	use osCommerce\OM\Core\OSCOM;

不是很清楚，特地翻了一下,use关键字是php5.3以上版本引入的

它的作用是给一个外部引用起别名。这是命名空间的一个重要特性，它同基于unix的文件系统的为文件或目录创建连接标志相类似。

PHP命名空间支持三种别名方式（或者说引用）：

1. 为一个类取别名
2. 为一个接口取别名
3. 为一个命名空间取别名

这三种方式都是用 use 关键字来完成。下面是三种别名的分别举例：

	//Example #1 importing/aliasing with the use operator
	<?php
	namespace foo;
	use My\Full\Classname as Another;

	// this is the same as use My\Full\NSname as NSname
	use My\Full\NSname;

	// importing a global class
	use ArrayObject;

	$obj = new namespace\Another; // instantiates object of class foo\Another
	$obj = new Another; // instantiates object of class My\Full\Classname
	NSname\subns\func(); // calls function My\Full\NSname\subns\func
	$a = new ArrayObject(array(1)); // instantiates object of class ArrayObject
	// without the "use ArrayObject" we would instantiate an object of class foo\ArrayObject
	?>

注意的一点是，对于已命名的名字,全称就包含了分隔符，比如 Foo\Bar,而不能用FooBar，而“\Foo\Bar”这个头部的"\"是没必要的，也不建议这样写。引入名必须是全称，并且跟当前命名空间没有程序上的关联。

PHP也可以在同一行上申明多个,等同于上面的写法

	<?php
	use My\Full\Classname as Another, My\Full\NSname;

	$obj = new Another; // instantiates object of class My\Full\Classname
	NSname\subns\func(); // calls function My\Full\NSname\subns\func
	?>

还有值得一说的是，引入是在编译时执行的，因此，别名不会影响动态类，例如：

	<?php
	use My\Full\Classname as Another, My\Full\NSname;

	$obj = new Another; // instantiates object of class My\Full\Classname
	$a = 'Another';
	$obj = New $a;     // instantiates object of class Another
	?>

这里由于给变量$a 赋值了 'Another'，编译的时候，就将$a 定位到 Classname 了。

文档中还有更详细的说明，篇幅有限，就说到这...

详细参考：http://php.net/manual/en/language.namespaces.importing.php
