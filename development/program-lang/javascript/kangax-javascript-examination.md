kangax 的javascript谜题
=======================

### 第一题

    (function(){ 
          return typeof arguments;
        })();
    //问自动执行函数会返回什么值
    // 就是考Arguments对象的typeof
    // 看平时用firebug多不多了……

### 第二题

    var f = function g(){ return 23; };
      typeof g();
    //问最后一行的执行结果
    //根据标准，命名函数表达式的函数名只对函数体内可见
    //因此报错

### 第三题

       (function(x){
        delete x;
        return x;
        })(1);
    //问自动执行函数会返回什么值
    // 参数不可删除
    //1

### 第四题

      var y = 1, x = y = typeof x;
        x;
    //问最后一行的执行结果
    //声明两个变量x与y，y最初赋为1,x没有赋值，默认赋给window的一个属性undefined，
    //因此typeof undefined为"undefined",最后x= y= "undefined"

### 第五题

      (function f(f){
        return typeof f();
        })(function(){ return 1; });
    //问自动执行函数会返回什么值
    //函数名被优先级更高的参数名覆盖了 --->
    // (function (f){
    //   return typeof f();
    //  })(function(){ return 1; });
    //typeof 1 ---> "number"

### 第六题

        var foo = { 
          bar: function() { return this.baz; }, 
          baz: 1
        };
        (function(){ 
          return typeof arguments[0]();
        })(foo.bar);
    //问自动执行函数会返回什么值
    //我们把下面那个自动执行函数分解一下
    //var a = function(){
    //  return typeof arguments[0]();
    //};
    //a(foo.bar)
    //执行完arguments[0]()，即得到this.baz
    //由于this变量在此绑定失效，它指向window，window有bax变量吗？
    //没有，返回"undefined"

### 第七题

        var foo = {
          bar: function(){ return this.baz; },
          baz: 1
        }
        typeof (f = foo.bar)();
    //问最后一行的执行结果
    //我们把最后一行分解一下
    //window.f
    //f= foo.bar
    // f()
    // typeof f()
    //返回"undefined"

### 第八题

       var f = (function f(){ return "1"; }, function g(){ return 2; })();
        typeof f;
    //问最后一行的执行结果
    //首先要理解分组选择符，最后a会赋给什么呢？
    // var a = (1,2,3);
    // alert(a) ---> 3
    //那么这就简单了，f = function(){return 2};
    //typeof f() ---> number

### 第九题

    var x = 1;
     if (function f(){}) {
       x += typeof f;
     }
     x;//问x的值
    //函数声明只能裸露于全局作用域下或位于函数体中。
        //从句法上讲，它们不能出现在块中，例如不能出现在
        // if、while 或 for 语句中。因为块只能包含语句，
        //因此if()中的f函数不能当做函数声明，当成表达式使用
        //可能在预编译阶段做了如下处理
        //if((XXX = function(){}))
        //因此我们是找不到f的
        // 1undefined

### 第十题

         var x = [typeof x, typeof y][1];
         typeof typeof x;
    //问最后一行的执行结果
    //数组其实就是这个样子["undefined","undefined"]
    //"string"

### 第十一题

       (function(foo){
          return typeof foo.bar;
        })({ foo: { bar: 1 } });
    //问自动执行函数会返回什么值
    //分解一下
    // var bb = { foo: { bar: 1 } }
    //  (function(j){
    //    return typeof j.bar
    // })(bb)
    // "undefined"
    //注意那个对象只有foo属性，没有bar属性

### 第十二题

        (function f(){
          function f(){ return 1; }
          return f();
          function f(){ return 2; }
        })();
    //问自动执行函数会返回什么值
    //函数声明会在预编译阶段被提前
    //2

### 第十三题

    function f(){ return f; }
     new f() instanceof f;//问这一行的值
    //由于函数f会返回自身，这个new 就形同虚设
    //如果f的形式为 function f(){return this}或function f(){}就不一样
    //false

### 第十四题

    with (function(x, undefined){}) length;
    //问length的值为多少
    //with就是一个读写器，题意是取出函数的length属性
    //而函数的length就是指它形参的长度  
     //2
