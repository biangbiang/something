解析PHP中ob_start()函数的用法
=============================

ob\_start()函数用于打开缓冲区,比如header()函数之前如果就有输出,包括回车/空格/换行/都会有"Header had all ready send by"的错误,这时可以先用ob\_start()打开缓冲区PHP代码的数据块和echo()输出都会进入缓冲区而不会立刻输出.当然打开缓冲区的作用很多,只要发挥你的想象.可以总结以下四点:

### 1.用于header()之前

    ob\_start(); //打开缓冲区 
    echo /"Hellon/"; //输出 
    header("location:index.php"); //把浏览器重定向到index.php 
    ob\_end\_flush();//输出全部内容到浏览器 
    ?>

### 2.phpinfo()函数可获取客户端和服务器端的信息,但要保存客户端信息用缓冲区的方法是最好的选择.

    ob\_start(); //打开缓冲区 
    phpinfo(); //使用phpinfo函数 
    $info=ob\_get\_contents(); //得到缓冲区的内容并且赋值给$info 
    $file=fopen(/'info.txt/',/'w/'); //打开文件info.txt 
    fwrite($file,$info); //写入信息到info.txt 
    fclose($file); //关闭文件info.txt 
    ?>

### 3.静态页面技术

    ob\_start();//打开缓冲区 
    ?> 
    php页面的全部输出 
    $content = ob\_get\_contents();//取得php页面输出的全部内容 
    $fp = fopen("output00001.html", "w"); //创建一个文件，并打开，准备写入 
    fwrite($fp, $content); //把php页面的内容全部写入output00001.html，然后…… 
    fclose($fp); 
    ?>

### 4.输出代码

    Function run\_code($code) { 
    If($code) { 
    ob\_start(); 
    eval($code); 
    $contents = ob\_get\_contents(); 
    ob\_end\_clean(); 
    }else { 
    echo "错误！没有输出"; 
    exit(); 
    } 
    return $contents; 
    }
