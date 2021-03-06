php数组
======

### Array 数组

PHP 中的数组实际上是一个有序映射。映射是一种把 values 关联到 keys 的类型。此类型在很多方面做了优化，因此可以把它当成真正的数组，或列表（向量），散列表（是映射的一种实现），字典，集合，栈，队列以及更多可能性。由于数组元素的值也可以是另一个数组，树形结构和多维数组也是允许的。

自 5.4 起可以使用短数组定义语法，用 [] 替代 array()。

一个简单数组声明：

    <?php
    $array = array(
      "foo" => "bar",
      "bar" => "foo",
    );

    // 自 PHP 5.4 起
    $array = [
      "foo" => "bar",
      "bar" => "foo",
    ];
    ?>

---

### 数组排序

array_reverse(PHP 4, PHP 5)

array_reverse — 返回一个单元顺序相反的数组

说明

    array array_reverse ( array $array [, bool $preserve_keys ] )

array_reverse() 接受数组 array 作为输入并返回一个单元为相反顺序的新数组，如果 preserve_keys 为 TRUE 则保留原来的键名。

---

### reset()

reset() 函数把数组的内部指针指向第一个元素，并返回这个元素的值。

若失败，则返回 FALSE。

---

### next()

next() 函数把指向当前元素的指针移动到下一个元素的位置，并返回当前元素的值。

如果内部指针已经超过数组的最后一个元素，函数返回 false。

---

### current()

current() 函数返回数组中的当前元素（单元）。

每个数组中都有一个内部的指针指向它“当前的”元素，初始指向插入到数组中的第一个元素。

current() 函数返回当前被内部指针指向的数组元素的值，并不移动指针。如果内部指针指向超出了单元列表的末端，current() 返回 FALSE。

---

### json转换

* json_encode():对变量进行JSON编码
* json_decode():对JSON格式的字符串进行解码，当第二个参数为TRUE时，直接可以转成array，而不会是对象

---

### array_intersect()

array_intersect() 函数返回两个或多个数组的交集数组。

结果数组包含了所有在被比较数组中，也同时出现在所有其他参数数组中的值，键名保留不变。

注释：仅有值用于比较。

语法：

    array_intersect(array1,array2,array3...)

---

### array_keys()

**定义和用法**

array_keys() 函数返回包含数组中所有键名的一个新数组。

如果提供了第二个参数，则只返回键值为该值的键名。

如果 strict 参数指定为 true，则 PHP 会使用全等比较 (===) 来严格检查键值的数据类型。

**语法**

	array_keys(array,value)

	参数	描述
	array	必需。规定输入的数组。
	value	可选。指定值的索引（键）。
	strict  可选。与 value 参数一起使用。可能的值：
		true - 根据类型返回带有指定值的键名。
		false - 默认值。不依赖类型。

**例子 1**

	<?php
	$a=array("a"=>"Horse","b"=>"Cat","c"=>"Dog");
	print_r(array_keys($a));
	?>

输出：

	Array ( [0] => a [1] => b [2] => c )

**例子 2**

使用 value 参数：

	<?php
	$a=array("a"=>"Horse","b"=>"Cat","c"=>"Dog");
	print_r(array_keys($a,"Dog"));
	?>

输出：

	Array ( [0] => c)

**例子 3**

使用 strict 参数 (false)：

	<?php
	$a=array(10,20,30,"10");
	print_r(array_keys($a,"10",false));
	?>

输出：

	Array ( [0] => 0 [1] => 3 )

**例子 4**

使用 strict 参数 (true)：

	<?php
	$a=array(10,20,30,"10");
	print_r(array_keys($a,"10",true));
	?>

输出：

	Array ( [0] => 3)
