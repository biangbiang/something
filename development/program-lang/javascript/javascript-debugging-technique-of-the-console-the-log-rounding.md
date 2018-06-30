# JavaScript调试技巧之console.log()详解

对于JavaScript程序的调试，相比于alert()，使用console.log()是一种更好的方式，原因在于：alert()函数会阻断JavaScript程序的执行，从而造成副作用；

alert弹出框需要点击确认比较麻烦，而console.log()仅在控制台中打印相关信息，因此不会造成类似的顾虑。

最重要的是alert只能输出字符串，不能输出对象里面的结构，console.log()console.log()可以接受任何字符串、数字和JavaScript对象，可以看到清楚的对象属性结构，在ajax返回json数组对象时调试很方便。

    //兼容Firefox/IE/Opera使用console.log
    if(!window.console){
    window.console = {log : function(){}};
    }
    window.console = window.console || {}; 
    console.log || (console.log = opera.postError);

下面分享两张打印出来的信息图片：

![](http://biang.io/biangpic/blog/6a590eae184a9925fdd82ef2b1ea7384.png)

php上传多文件console.log打印日志

![](http://biang.io/biangpic/blog/68cc6d32ad5584f67b8a0aa1d5d7ddd4.png)

console.log 原先是 Firefox 的"专利"，严格说是安装了 Firebugs 之后的 Firefox 所独有的调试"绝招"。

IE8用起来比 Firebugs 麻烦，只有在开启调试窗口(F12)的时候，console.log 才能出结果，不然就报错。 

IE浏览器下默认是不支持console.log，反而会因为这句代码报错，解决办法是声明该console对象的log函数为空函数

    if(!window.console){
    window.console = {log : function(){}};
    }

Opera还是不能用 console.log 加上下面两句代码兼容： 

    window.console = window.console || {}; 
    console.log || (console.log = opera.postError);

这样Firefox/IE/Opera 都能用上console.log，不过没有必要去做这种兼容性工作 console.log()等调试代码应当从最终的产品代码中删除掉。

IE 和 Opera 下的 console.log 比起 Firebugs还是太过简单，比如参数是 Object 或者数组就没有进一步的显示功能。

    //变量 
    var i = 'I am a string'; 
    console.log('变量：',i); 
    //数组 
    var arr = [1,2,3,4,5]; 
    console.log('数组：',arr); 
    //对象 
    var obj1 = { 
    key1 : 'value1', 
    key2 : 'value2', 
    key3 : 'value3' 
    }; 
    var obj2 = { 
    key6 : 'value4', 
    key5 : 'value5', 
    key4 : 'value6' 
    }; 
    var obj3 = { 
    key9 : 'value7', 
    key8 : 'value8', 
    key7 : 'value9' 
    }; 
    console.log('对象：',obj1); 
    //对象数组 
    var objArr1 = [obj1,obj2,obj3]; 
    var objArr2 = [[obj1],[obj2],[obj3]]; 
    console.log('对象数组1：',objArr1); 
    console.log('对象数组1：',objArr2); 
    /* 
    输出： 
    变量：I am a string 
    数组：[1, 2, 3, 4, 5] 
    对象：Object { key1="value1", key2="value2", key3="value3"} 
    对象数组1：[Object { key1="value1", key2="value2", key3="value3"}, Object { key6="value4", key5="value5", key4="value6"}, Object { key9="value7", key8="value8", key7="value9"}] 
    对象数组1：[[Object { key1="value1", key2="value2", key3="value3"}], [Object { key6="value4", key5="value5", key4="value6"}], [Object { key9="value7", key8="value8", key7="value9"}]] 
    */

除了console.log()，Firebug还支持多种不同的日志级别：debug、info、warn、error。以下代码将在控制台中打印这些不同日志级别的信息：

代码如下://Use different logging level

    console.log("Log level");
    console.debug("Debug level");
    console.info("Info level");
    console.warn("Warn level");
    console.error("Error level");

从Firebug控制台中可以看到，不同日志级别的打印信息，其颜色和图标是不一样的；同时，可以在控制台中选择不同的日志级别来对这些信息进行过滤，不过这些比较少用到，不是很实用。
