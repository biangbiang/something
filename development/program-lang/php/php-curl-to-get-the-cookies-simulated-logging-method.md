# PHP CURL获取cookies模拟登录的方法

要提取google搜索的部分数据，发现google对于软件抓取它的数据屏蔽的厉害，以前伪造下 USER-AGENT 就可以抓数据，但是现在却不行了。利用抓包数据发现，Google 判断了 cookies，当你没有cookies的时候，直接返回 302 跳转，而且是连续几十个302跳转，根本抓不了数据。

因此，在发送搜索命令时，需要先提取 cookies 并保存，然后利用保存下来的这个cookies再次发送搜索命令即可正常抓数据了。这其实和论坛的模拟登录一个道理，先POST登录，获取cookies并保存，然后利用这个cookies访问就可以了。

PHP 代码如下：

    <?php
    header('Content-Type: text/html; charset=utf-8');

    $cookie_file = dirname(__FILE__).'/cookie.txt';
    //$cookie_file = tempnam("tmp","cookie");

    //先获取cookies并保存
    $url = "http://www.google.com.hk";
    $ch = curl_init($url); //初始化
    curl_setopt($ch, CURLOPT_HEADER, 0); //不返回header部分
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); //返回字符串，而非直接输出
    curl_setopt($ch, CURLOPT_COOKIEJAR,  $cookie_file); //存储cookies
    curl_exec($ch);
    curl_close($ch);

    //使用上面保存的cookies再次访问
    $url = "http://www.google.com.hk/search?oe=utf8&ie=utf8&source=uds&hl=zh-CN&q=qq";
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_COOKIEFILE, $cookie_file); //使用上面获取的cookies
    $response = curl_exec($ch);
    curl_close($ch);

    echo $response;
    ?>
