ECMAScript 迭代语句
===================

迭代语句又叫循环语句，声明一组要反复执行的命令，直到满足某些条件为止。

循环通常用于迭代数组的值（因此而得名），或者执行重复的算术任务。

本节为您介绍 ECMAScript 提供的四种迭代语句。

---

### do-while 语句

do-while 语句是后测试循环，即退出条件在执行循环内部的代码之后计算。这意味着在计算表达式之前，至少会执行循环主体一次。

它的语法如下：

	do {statement} while (expression);

例子：

	var i = 0;
	do {i += 2;} while (i < 10);

---

### while 语句

while 语句是前测试循环。这意味着退出条件是在执行循环内部的代码之前计算的。因此，循环主体可能根本不被执行。

它的语法如下：

	while (expression) statement

例子：

	var i = 0;
	while (i < 10) {
	  i += 2;
	}

---

### for 语句

for 语句是前测试循环，而且在进入循环之前，能够初始化变量，并定义循环后要执行的代码。

它的语法如下：

	for (initialization; expression; post-loop-expression) statement

注意：post-loop-expression 之后不能写分号，否则无法运行。

例子：

	iCount = 6;
	for (var i = 0; i < iCount; i++) {
	  alert(i);
	}

这段代码定义了初始值为 0 的变量 i。只有当条件表达式（i < iCount）的值为 true 时，才进入 for 循环，这样循环主体可能不被执行。如果执行了循环主体，那么将执行循环后表达式，并迭代变量 i。

---

### for-in 语句

for 语句是严格的迭代语句，用于枚举对象的属性。

它的语法如下：

	for (property in expression) statement

例子：

	for (sProp in window) {
	  alert(sProp);
	}

这里，for-in 语句用于显示 window 对象的所有属性。

前面讨论过的 PropertyIsEnumerable() 是 ECMAScript 中专门用于说明属性是否可以用 for-in 语句访问的方法。
