php中的urlencode()、urldecode() 的使用方法
==========================================

在PHP中有urlencode()、urldecode()、rawurlencode()、rawurldecode()这些函数来解决网页URL编码解码问题。在ASP的时候URL编码解码很是恼火，Server.urlencode不太好用，遇到utf-8编码的地址更是麻烦。你要获取百度、Google点击到网站的网址链接中的关键字，要写上一堆自定义函数来得到urldecode的效果。摘录一篇关于PHP urlencode()函数的文章，对PHP处理URL作全面了解。

### 理解urlencode

urlencode：是指针对网页url中的中文字符的一种编码转化方式，最常见的就是Baidu、Google等搜索引擎中输入中文查询时候，生成经过 Encode过的网页URL。urlencode的方式一般有两种一种是传统的基于GB2312的Encode（Baidu、Yisou等使用），一种是 基于utf-8的Encode(Google,Yahoo等使用)。本工具分别实现两种方式的Encode与Decode。

    中文 -> GB2312的Encode -> %D6%D0%CE%C4
    中文 -> utf-8的Encode -> %E4%B8%AD%E6%96%87

### Html中的urlencode

编码为GB2312的html文件中：

    http://lanmeng.org/中文.rar -> 浏览器自动转换为 -> http://www.lanmeng.org/%D6%D0%CE%C4.rar

注意：Firefox对GB2312的Encode的中文URL支持不好，因为它默认是utf-8编码发送URL的，但是ftp://协议可以，应该算是Firefox一个bug。

编码为utf-8的html文件中：

    http://lanmeng.org/中文.rar -> 浏览器自动转换为 -> http://www.lanmeng.org/%E4%B8%AD%E6%96%87.rar

PHP中的urlencode：

    <?php
    //GB2312的Encode
    echo urlencode("中文-_. ")."\n"; //%D6%D0%CE%C4-_.+
    echo urldecode("%D6%D0%CE%C4-_. ")."\n"; //中文-_.
    echo rawurlencode("中文-_. ")."\n"; //%D6%D0%CE%C4-_.%20
    echo rawurldecode("%D6%D0%CE%C4-_. ")."\n"; //中文-_.
    ?>

除了 -_. 之外的所有非字母数字字符都将被替换成百分号（%）后跟两位十六进制数。

### urlencode和rawurlencode的区别

urlencode 将空格则编码为加号（+）

rawurlencode 将空格则编码为加号（%20）

### utf-8的Encode两种方法

一、将文件存为utf-8文件，直接使用urlencode、rawurlencode即可。

二、使用mb_convert_encoding函数。

    <?php
    $url = 'http://www.lanmeng.org/中文.rar';
    echo urlencode(mb_convert_encoding($url, 'utf-8', 'gb2312'))."\n";
    echo rawurlencode(mb_convert_encoding($url, 'utf-8', 'gb2312'))."\n";
    //http%3A%2F%2Fwww.lanmeng.org%2F%E4%B8%AD%E6%96%87.rar
    ?>

应用实例：

    function parseurl($url="")
    {
    $url = rawurlencode(mb_convert_encoding($url, 'gb2312', 'utf-8'));
    $a = array("%3A", "%2F", "%40");
    $b = array(":", "/", "@");
    $url = str_replace($a, $b, $url);
    return $url;
    }
    $url="ftp://fufu:password@www.lanmeng.org/中文/中文.rar";
    echo parseurl($url);
    //ftp://fufu:password@www.lanmeng.org/%D6%D0%CE%C4/%D6%D0%CE%C4.rar
    ?>
