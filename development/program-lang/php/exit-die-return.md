PHP中exit,exit(0),exit(1),exit('0'),exit('1'),die,return的区别
==============================================================

die('1')  die()和exit()都是中止脚本执行函数；其实exit和die这两个名字指向的是同一个函数，die()是exit()函数的别名。该函数只接受一个参数，可以是一个程序返回的数值或是一个字符串，也可以不输入参数，结果没有返回值。

参考：虽然两者相同，但通常使用中也有细微的选择性。

当传递给exit和die函数的值为0时，意味着提前终止脚本的执行，通常用exit()这个名字。

    echo "1111";
    exit(0);
    echo "2222";

当程序出错时，可以给它传递一个字符串，它会原样输出在系统终端上，通常使用die()这个名字。

    $fp=fopen("./readme.txt","r") or die("不能打开该文件");
    //这种情况下，如果fopen函数被调用返回布尔值false时，die()将立即终止脚本，并马上打印
    //传递给它的字符串，“死前还能说一两句话”。

同样的die('1')也通exit('1')一样，输出1

    echo "begin";
    die('1');
    echo "end";
    //输出begin1

exit(1) 不输出内容，结束程序

    echo "begin";
    exit(1);
    echo "end";
    //输出begin

exit(0) 不输出内容，结束程序

    echo "begin";
    exit(0);
    echo "end";
    //输出begin

exit('0') 输出0 并结束程序

    echo "begin";
    exit('0');
    echo "end";
    //输出begin0

exit('1') 输出1 并结束程序

    echo "begin";
    exit('1');
    echo "end";
    //输出begin1

return 返回值，后续的程序也不执行，值并不输出

    echo "begin";
    return 1;
    echo "end";
    //输出begin,return的值没有输出到屏幕,而是返回给了上一层

总结：

* return是返回值
* die是遇到错误才停止
* exit是直接停止，并且不运行后续代码，exit()可以显示内容。
* return就是纯粹的返回值了，但是也不会运行后续代码
* exit（0）：正常运行程序并退出程序；
* exit（1）：非正常运行导致退出程序；
