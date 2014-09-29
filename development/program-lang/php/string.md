php字符串
========

### substr()

substr() 函数返回字符串的一部分。

substr(string,start,length)

本函数将字符串 string 的第 start 位起的字符串取出 length 个字符。若 start 为负数，则从字符串尾端算起。若可省略的参数 length 存在，但为负数，则表示取到倒数第 length 个字符。

    <?
      echo substr("abcdef", 1, 3); // 返回 "bcd"
      echo substr("abcdef", -2); // 返回 "ef"
      echo substr("abcdef", -3, 1); // 返回 "d"
      echo substr("abcdef", 1, -1); // 返回 "bcde"
    ?>

### explode()

explode() 函数把字符串分割为数组。

explode(separator,string,limit)

* separator：必需。规定在哪里分割字符串。
* string：必需。要分割的字符串。
* limit：可选。规定所返回的数组元素的最大数目。

说明：

* 本函数返回由字符串组成的数组，其中的每个元素都是由 separator 作为边界点分割出来的子字符串。
* separator 参数不能是空字符串。如果 separator 为空字符串（""），explode() 将返回 FALSE。如果 separator 所包含的值在 string 中找不到，那么 explode() 将返回包含 string 中单个元素的数组。
* 如果设置了 limit 参数，则返回的数组包含最多 limit 个元素，而最后那个元素将包含 string 的剩余部分。
* 如果 limit 参数是负数，则返回除了最后的 -limit 个元素外的所有元素。此特性是 PHP 5.1.0 中新增的。

注意：

由于历史原因，虽然 implode() 可以接收两种参数顺序，但是 explode() 不行。你必须保证 separator 参数在 string 参数之前才行。

### implode()

implode() 函数把数组元素组合为一个字符串。

implode(separator,array)

* separator：可选。规定数组元素之间放置的内容。默认是 ""（空字符串）。
* array：必需。要结合为字符串的数组。

说明：

虽然 separator 参数是可选的。但是为了向后兼容，推荐您使用使用两个参数。

### stripos()和strpos()


### stristr()和strstr()
