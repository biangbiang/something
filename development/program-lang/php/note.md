php学习笔记
=========

### 官方文档

文档地址：http://php.net/manual/en/index.php

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

### 几个 PHP 的“魔术常量”

    名称	说明
    __LINE__    文件中的当前行号。
    __FILE__    文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径。
    __DIR__	    文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。它等价于 dirname(__FILE__)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增） =
    __FUNCTION__    函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。
    __CLASS__   类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 Foo\Bar）。注意自 PHP 5.4 起 __CLASS__ 对 trait 也起作用。当用在 trait 方法中时，__CLASS__ 是调用 trait 方法的类的名字。
    __TRAIT__   Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4 起此常量返回 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）。
    __METHOD__  类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。
    __NAMESPACE__   当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。

### 超全局变量

    超全局变量 — 超全局变量是在全部作用域中始终可用的内置变量
    $GLOBALS — 引用全局作用域中可用的全部变量
    $_SERVER — 服务器和执行环境信息
    $_GET — HTTP GET 变量
    $_POST — HTTP POST 变量
    $_FILES — HTTP 文件上传变量
    $_REQUEST — HTTP Request 变量
    $_SESSION — Session 变量
    $_ENV — 环境变量
    $_COOKIE — HTTP Cookies
    $php_errormsg — 前一个错误信息
    $HTTP_RAW_POST_DATA — 原生POST数据
    $http_response_header — HTTP 响应头
    $argc — 传递给脚本的参数数目
    $argv — 传递给脚本的参数数组

### php\_sapi\_name

php\_sapi\_name — 返回 web 服务器和 PHP 之间的接口类型

返回接口类型的小写字符串。

尽管不够全面，可能返回的值包括了 aolserver、apache、 apache2filter、apache2handler、 caudium、cgi （直到 PHP 5.3）, cgi-fcgi、cli、 cli-server、 continuity、embed、fpm-fcgi、 isapi、litespeed、 milter、nsapi、 phttpd、pi3web、roxen、 thttpd、tux 和 webjames。

### 一些PHP函数

* `dirname()`：函数返回路径中的目录部分
* `chdir()`：改变目录
* `is_file()`：判断给定文件名是否为一个正常的文件
* `clearstatcache()`：清除文件状态缓存
* `parse_url()`：解析 URL，返回其组成部分

---

### 引用传递、引用返回和取消引用以及unset

引用传递

    function foo ( &$var )
    {$var++;}
    foo ($a);  // 注意在函数调用时没有引用符号 － 只有函数定义中有。光是函数定义就足够使参数通过引用来正确传递了

引用返回

    function &init_users()
    { ... return $cls;}

使用引用返回，必须在两个地方都用&符号

    $user = & init_users();
    function &init_users()
    { ...return $cls;}

取消引用

当 unset 一个引用，只是断开了变量名和变量内容之间的绑定。这并不意味着变量内容被销毁了。

    $a="hihaha";
    $b= &$a;
    unset($b);
    echo$a;// shows "hihaha"

PHP unset全局变量在用户函数中只能销毁局部变量，并不能销毁全局变量。（从PHP4开始unset已经不再是一个函数了，而是一个语句）。如果需要销毁全局变量的应该如何做呢？也很简单，用$GLOBALS数组来实现。

    < ?PHP 
    function foo() { 
    unset($GLOBALS['bar']);  // 而不是unset($bar)
    } 
    $bar = “something”; 
    foo(); 
    var_dump($bar); 
    ?>

对于unset：

1. 该函数只有在变量值所占空间超过256字节长的时候才会释放内存

2. 只有当指向该值的所有变量（比如有引用变量指向该值）都被销毁后，地址才会被释放（也要执行1的判断）

也就是检查有无其他变量绑定，有的话就不会释放了。就像这个例子：

    $a="hihaha";
    $b= &$a;
    unset($b);
    echo$a;// shows "hihaha"

所以建议大家用 $变量=null 的方法来释放其内存。

给一个测试当前php脚本内存使用情况的函数：

    <?php
    echo memory_get_usage()."\n";
    $a = str_repeat("A", 1000);
    echo memory_get_usage()."\n";
    $b=&$a;  //下面的内存大小不会变，unset只是解除$a的绑定
    unset($a);
    echo memory_get_usage()."\n";
    ?>

### htmlspecialchars\_decode

htmlspecialchars\_decode — 将特殊的 HTML 实体转换回普通字符

flags

用下列标记中的一个或多个作为一个位掩码，来指定如何处理引号和使用哪种文档类型。默认为 `ENT_COMPAT | ENT_HTML401`。

有效的 flags 常量

    常量名	说明
    ENT_COMPAT	转换双引号，不转换单引号。
    ENT_QUOTES	单引号和双引号都转换。
    ENT_NOQUOTES	单引号和双引号都不转换。
    ENT_HTML401	作为HTML 4.01编码处理。
    ENT_XML1	作为XML 1编码处理。
    ENT_XHTML	作为XHTML编码处理。
    ENT_HTML5	作为HTML 5编码处理。

实际工作中，必须了解flag的设置，默认值往往不能达到期望的效果。
