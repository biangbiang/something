# js字符串转换为Json对象的三种写法

在进行web前端开发时，经常会遇到将js字符串转换成Json对象的情况，这种转换有三种写法：

1.使用eval()来进行解析（最原始的写法，但却很有效）

    function strToJson(str){ 
        var json = eval('(' + str + ')'); 
        return json; 
    }

这种写法适合从数据库取出json字符串，然后需要进行转换为json对象的方式。

2.使用new function()的方式

    function strToJson(str){ 
        var json = (new Function("return " + str))(); 
        return json; 
    }

3.使用JSON的转换方法

    function strToJson(str){ 
        return JSON.parse(str); 
    }

这种的转换方式对应json字符串的要求比较严谨，一定要完全符合json的写法，属性都要用“”双引号引起来，否则会出现解析异常。如：

    var str = '{name:"jack"}'; 
    var obj = JSON.parse(str); // --> parse error

正确的写法：

    var str = '{“name”:"jack"}'; 
    var obj = JSON.parse(str); // --> parse success
