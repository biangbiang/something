# 关于PHP中strtotime方法返回负数时间戳的一个坑

最近在做云签到，其中一个功能是判断用户是否过期，如果已过期则不进行签到。虽然PHP有个很强大的DateTime类，但直觉告诉我用这个类的开销会比较大，那么替代的方案自然就是用strtotime将日期转为时间戳再进行对比

数据库内记录的日期并非都是有效日期，对于一些用户我会将日期留空（即0000-00-00）表示永久有效。根据文档及本地实际测试，strtotime('0000-00-00')会返回false。因此便有了如下代码：

    $expire = （从数据库取出的用户有效期）;
    $today = date('Y-m-d');
    $expire = strtotime($expire);
    $today = strtotime($today);
    if($expire === FALSE || $expire > $today){
        echo '账号未过期';
    } else {
        echo '账号已过期';
    }
    // 以上代码仅作演示，实际上当然不会这么写

似乎没什么问题，但扔服务器上实际一运行，却将所有有效期为0000-00-00的用户都判定为过期了，为啥呢！？

本地测试，完美运行。

为毛啊！？

后来经过耐心调试，发现在服务器上strtotime('0000-00-00')竟会返回负数的时间戳！这不科学，PHP官方文档中并没有提到这点啊？我继续往下拉，就发现了某网友的留言：

    You are not restricted to the same date ranges when running PHP on a 64-bit machine. This is because you are using 64-bit integers instead of 32-bit integers (at least if your OS is smart enough to use 64-bit integers in a 64-bit OS)
     
    The following code will produce difference output in 32 and 64 bit environments.
     
    var_dump(strtotime('1000-01-30'));
     
    32-bit PHP: bool(false)
    64-bit PHP: int(-30607689600)
     
    This is true for php 5.2.* and 5.3
     
    Also, note that the anything about the year 10000 is not supported. It appears to use only the last digit in the year field. As such, the year 10000 is interpretted as the year 2000; 10086 as 2006, 13867 as 2007, etc

即便是英语硬伤如我，也看懂其中的问题所在了

噢好吧，我还是将$expire <= 0判定为账号未过期吧，这下总算行了
