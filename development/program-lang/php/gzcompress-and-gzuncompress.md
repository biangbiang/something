压缩存储长字符串 gzcompress 和 gzuncompress 的使用
==================================================

导读：对于博客类系统，文章内容的存储，会带来数据库空间的大量消耗。进行压缩存储不失为一种好的方式。

详细：

处理方式很简单，直接代码：

    <?php
    $mysql = new mysqli('localhost', 'root', '', 'test');

    $msg = '我爱我家我爱我家我爱我家我爱我家我爱我家我爱我家我爱我家我爱我家我爱我';
    $sql_ins = "INSERT INTO log_20130204 (txt) VALUES ('" . gzcompress($msg) . "')";
    $mysql->query($sql_ins);

    $sql_sel = "SELECT * FROM log_20130204 WHERE uid=5";
    $result = $mysql->query($sql_sel);
    while ($row = $result->fetch_row()) {
        echo strlen($row[1]) . '<br>';
        printf("%d - %s", $row[0], gzuncompress($row[1]));
        echo '<br>' . strlen(gzuncompress($row[1]));
    }

这是在缺省情况下，使用默认的压缩等级6，有人对0~9各个等级进行了测试，结果如下：

    Compression benchmark: (level, time, size (%)):
    0: 0.000373 - 82.08 kB (100.02%)
    1: 0.000914 - 19.61 kB (23.90%)
    2: 0.000951 - 18.88 kB (23.01%)
    3: 0.000999 - 18.43 kB (22.46%)
    4: 0.001498 - 17.65 kB (21.51%)
    5: 0.001744 - 17.09 kB (20.82%)
    6: 0.002060 - 16.88 kB (20.57%)
    7: 0.002233 - 16.85 kB (20.53%)
    8: 0.002808 - 16.71 kB (20.36%)
    9: 0.002928 - 16.71 kB (20.36%)

可以看得出，性能和存储是需要根据实际情况选择一个平衡点的，上面的测试结果估计有一定的偏差，

最优的应该是缺省的6这个压缩等级。
