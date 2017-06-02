# 对匿名函数的深入理解（彻底版）

从简单的字面理解就是一个没有名字的函数，但是如果说它只是这样简单，那我也就没有必要来说这些。

### 对匿名函数的理解1：

    function(){
        console.log(1); 
    }
    // 报错

不能直接使用。

### 对匿名函数的理解2：

    var a = function(){
        console.log(1);
    }
    a(); //1

匿名函数可以依附于一个变量，并且这个变量名就是这个匿名函数的名字。

    var a = function(){
        console.log(1);
    }
    console.log(typeof a); //function

### 对匿名函数的理解3：

    window.onload = function(){
        console.log("匿名函数");
    }
    // 匿名函数

当匿名函数用在绑定事件中的时候，当这个事件执行的时候这个匿名函数也会被执行。

### 对匿名函数的理解4：

    (function(){
        console.log("追梦子");
    })()
    // 追梦子

如果将匿名函数放入到表达式中并且后面加上小括号会自动执行这个函数

### 对匿名函数的理解5：

    var a = function(){
        console.log("自执行函数");
    }();
    // 自执行函数

匿名函数后面加括号会执行这个函数。

### 对匿名函数的理解6：

    function(){
        console.log(1);
    }();
    // 报错

匿名函数必须依附一个变量。

### 对匿名函数的理解7：

    var a = {
        fn:function(){
            console.log(1);
        }
    }
    a.fn(); //1

匿名函数不只是可以依附于一个变量，也可以依附于一个对象的属性。

### 对匿名函数的理解8：

    var a = {
        fn:function(){
            console.log("追梦子");
        }()
    } //追梦子

同样的匿名函数当做一个对象的属性时也可以自调用。

### 对匿名函数的理解9：

    var a = function(b){
        console.log(b)
    }
    a(52); //52

匿名函数也可以传递参数

### 对匿名函数的理解10：

    (function(a){
        console.log(a);
    })(10) //10

对于表达式函数同样也可以传递参数

### 对匿名函数的理解11：

    var a = function(b){
        console.log(b);
    }(10); //10
    console.log(a); //undefined

如果将一个自执行的匿名函数并且没有返回值，赋值给一个变量那么这个变量的值就是undefined。因为这个函数在赋值之前已经执行完了，而这个函数没有返回值，所以就是undefined，如果有返回值，那么这个变量的值就是那个匿名函数的返回值。

    var a = function(b){
        return b;
    }(10);
    console.log(a); //10

### 这里对匿名函数的讲解就到此结束，下面说一下关于自执行的匿名函数问题。

why？为什么下面这段代码会报错？

    function(){
        console.log(1);
    }
    // 报错

1、因为ECAMScript规定函数的声明必须要有名字，如果没有名字的话，我们就没有办法找到它了，对于为什么自执行函数为什么就可以不带名字后面会讲。


2、如果没有名字必须要有一个依附体，如：将这个匿名函数赋值给一个变量。

如果按照上面的说法js报错也是应该的，那么我们用的下面这种代码为什么就能够正常运行？

    (function(){
        console.log(1);
    })() //1

之所以可以是因为我们将这个函数包含在了一个小括号中，why？小括号为什么这么神奇？

按照ECAMScript的规定，函数声明是必须要有名字的，但是我们用括号扩起来那么这个函数就不再是一个函数声明了，而是一个函数表达式，你可以理解成下面这段代码。

    var a = function(){
        console.log(1);
    }(); //1

将一个匿名函数赋值给一个变量或者对象属性就是函数表达式，函数表达式是可以不需要名字的，所以我们就可以直接通过这种方式来自动的执行这个函数。

再说一句

    (function(){
        ....
    })()

第一个括号是个运算符，它会返回这个匿名函数，然后最后一个小括号会执行这个函数。

总结也就是说只要是表达式就可以直接执行它，并且不需要函数名，那么我们还可以这样调用它。

    -function(){
        console.log(1);
    }()
    +function(){
        console.log(2);
    }()
    ~function(){
        console.log(3);
    }()
    !function(){
        console.log(4);
    }()
    &function(){
        console.log(5);
    }()
    |function(){
        console.log(6);
    }()
    /function(){
        console.log(7);
    }()
    /function(){
        console.log(8)
    }()
    %function(){
        console.log(9)
    }()
    typeof function(){
        console.log(10)
    }()
    null==function(){
        console.log(11)
    }()
    0==function(){
        console.log(12)
    }()
    0!=function(){
        console.log(13)
    }()
    0>function(){
        console.log(14)
    }()
    0<function(){
        console.log(15)
    }()
    0-function(){
        console.log(16)
    }()
