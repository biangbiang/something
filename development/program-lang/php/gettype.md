php 获取变量的类型
==================

## gettype

(PHP 4, PHP 5)

gettype — 获取变量的类型

### 描述

string gettype ( mixed $var )

返回 PHP 变量的类型 var.

### Warning

不要使用 gettype() 来测试某种类型，因为其返回的字符串在未来的版本中可能需要改变。此外，由于包含了字符串的比较，它的运行也是较慢的。

使用 is_* 函数代替。

返回的字符串的可能值为：

* “boolean”（从 PHP 4 起）
* “integer”
* “double”（由于历史原因，如果是 float 则返回“double”，而不是“float”）
* “string”
* “array”
* “object”
* “resource”（从 PHP 4 起）
* “NULL”（从 PHP 4 起）
* “user function”（只用于 PHP 3，现已停用）
* “unknown type”

对于 PHP 4，你应该使用 function\_exists() 和 method\_exists() 取代先前将 gettype() 作用于函数的用法。

参见 settype()、is\_array()、is\_bool()、is\_float()、is\_integer()、is\_null()、is\_numeric()、is\_object()、is\_resource()、is\_scalar() 和 is\_string()。
