# Nginx access.log日志分析shell命令

Nginx 版本信息:

    nginx version: nginx/0.8.53

Nginx日志配置项:

    access_log      /data0/logs/access.log  combined;

Nginx日志格式:

    $remote_addr – $remote_user [$time_local]  $request $status $apache_bytes_sent $http_referer $http_user_agent
    127.0.0.1 - - [24/Mar/2011:12:45:07 +0800] "GET /fcgi_bin/xxx.fcgi?id=xxx HTTP/1.0" 200 160 "-" "Mozilla/4.0"

通过日志查看当天访问页面排前10的url:

    #>cat access.log | grep "24/Mar/2011" | awk '{print $7}' | sort | uniq -c | sort -nr | head -n 10

通过日志查看当天ip连接数,统计ip地址的总连接数

    #>cat access.log | grep "24/Mar/2011" | awk '{print $1}' | sort | uniq -c | sort –nr
    38 112.97.192.16
    20 117.136.31.145
    19 112.97.192.31
    3 61.156.31.20
    2 209.213.40.6
    1 222.76.85.28

通过日志查看当天访问次数最多的10个IP ,只需要在上一个命令后加上head命令

    #>cat access.log | grep "24/Mar/2011" |awk '{print $3}'|sort |uniq -c|sort -nr|head –n 10
    38 112.97.192.16
    20 117.136.31.145
    19 112.97.192.31
    3 61.156.31.20
    2 209.213.40.6
    1 222.76.85.28

通过日志查看当天访问次数最多的10个IP

    #>awk '{print $1}' access.log |sort |uniq -c|sort -nr|head
    10680 10.0.21.17
    1702 10.0.20.167
    823 10.0.20.51
    504 10.0.20.255
    215 58.60.188.61
    192 183.17.161.216
    38 112.97.192.16
    20 117.136.31.145
    19 112.97.192.31
    6 113.106.88.10

通过日志查看当天指定ip访问次数过的url和访问次数:

    #>cat access.log | grep "10.0.21.17" | awk '{print $7}' | sort | uniq -c | sort –nr
    224 /test/themes/default/img/logo_index.gif
    224 /test/themes/default/img/bg_index_head.jpg
    224 /test/themes/default/img/bg_index.gif
    219 /test/vc.php
    219 /
    213 /misc/js/global.js
    211 /misc/jsext/popup.ext.js
    211 /misc/js/common.js
    210 /sladmin/home
    197 /misc/js/flib.js

通过日志查看当天访问次数最多的时间段

    #>awk '{print $4}' access.log | grep "24/Mar/2011" |cut -c 14-18|sort|uniq -c|sort -nr|head
    24 16:49
    19 16:17
    16 16:51
    11 16:48
    4 16:50
    3 16:52
    1 20:09
    1 20:05
    1 20:03
    1 19:55
