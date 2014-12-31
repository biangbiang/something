PHP 输出中文 JSON 字符串
========================

PHP 和 JavaScript 交互其实很方便，PHP 原生也提供了对 JSON 格式的支持。主要包括 JSON 编码和解码两个函数：

* json_endoce: http://cn.php.net/json_encode

* json_dedoce: http://cn.php.net/json_decode

json_encode — 对变量进行 JSON 编码，并返回 value 值的 JSON 形式，例如：

    <!--?php
    $arr = array ('a'=-->1,'b'=>2,'c'=>3,'d'=>4,'e'=>5);
    echo json_encode($arr);
    ?>

以上代码执行后输出：

    {"a":1,"b":2,"c":3,"d":4,"e":5}

假如要编码的数据源（一般是一个数组），value 中包含中文，经过 json_encode 处理后输出的是 unicode 编码。

    <!--?php
    $arr = array ('a'=-->'WEB编程技术交流网');
    echo json_encode($arr);
    ?>

以上代码执行后输出：

    {"a":"\u8292\u679C\u5C0F\u7AD9"}

PHP 底层已经做了 unicode 处理，如果嫌它不够直观，可以利用 urlencode 和 urldecode 方法绕过这个转码为 unicode 的过程：

    $arr = array ('a'=>urlencode('WEB编程技术交流网'));
    echo urldecode(json_encode($arr));

以上代码执行后输出：

    {"a":"WEB编程技术交流网"}
