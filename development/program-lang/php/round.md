php中的四舍五入函数
===================

php 中处理浮点数时经常要需要四舍五入。在php 中有两个函数适用于这种情况：floor函数、ceil函数和round函数。

floor函数和ceil函数互相搭配起来可以使php 处理的数据更加真实可靠。

### 一、先来看floor函数：

语法：

	float floor ( float value )

说明：

返回不大于 value 的下一个整数，将 value 的小数部分舍去取整。floor() 返回的类型仍然是 float，因为 float 值的范围通常比 integer 要大。

floor() 例子

	<?php
	  echo floor(1.6);  // will output "1"
	  echo floor(-1.6); // will output "-2"
	?>



### 二、ceil函数：

语法：

	float ceil ( float value )

说明：

返回不小于 value 的下一个整数，value 如果有小数部分则进一位。ceil() 返回的类型仍然是 float，因为 float 值的范围通常比 integer 要大。

ceil() 例子：

	<?php
	echo ceil(4.3);    // 5
	echo ceil(9.999);  // 10
	echo ceil(-3.14);  // -3
	?>

看到这两个函数的区别了么。。

当然，如果需要制定精度就需要使用round函数了。

### 三、round函数：

语法：

	float round ( float val [, int precision] )

说明：

返回将 val 根据指定精度 precision（十进制小数点后数字的数目）进行四舍五入的结果。precision 也可以是负数或零（默认值）。

round() 例子

	<?php
	echo round(3.4);         // 3
	echo round(3.5);         // 4
	echo round(3.6);         // 4
	echo round(3.6, 0);      // 4
	echo round(1.95583, 2);  // 1.96
	echo round(1241757, -3); // 1242000
	echo round(5.045, 2);    // 5.05
	echo round(5.055, 2);    // 5.06
	?>

