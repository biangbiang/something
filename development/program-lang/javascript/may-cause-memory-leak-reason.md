容易造成JavaScript内存泄露几个方面
=================================

这篇文章主要介绍了容易造成JavaScript内存泄露几个方面,本文讲解了多个会在Chrome V8中产生内存泄漏的示例,需要的朋友可以参考下

---

高效的JavaScript Web应用必须流畅，快速。与用户交互的任何应用程序，都需要考虑如何确保内存有效使用，因为如果消耗过多，页面就会崩溃，迫使用户重新加载。而你只能躲在角落哭泣。

自动垃圾收集是不能代替有效的内存管理的，特别是在大型，长时间运行的Web应用程序中。在这次讲座中，我们将演示如何通过Chrome的DevTools对内存进行有效的管理。

并了解如何解决性能问题，如内存泄漏，频繁的垃圾收集暂停，和整体内存膨胀，那些真正让你耗费精力的东西。

Addy Osmani在他的PPT中展示了很多会在Chrome V8中产生内存泄漏的示例：

### 1) Delete一个Object的属性会让此对象变慢（多耗费15倍的内存）

    var o = { x: 'y' };
    delete o.x; //此时o会成一个慢对象
    o.x; //
    var o = { x: 'y' };
    o = null;  //应该这样

### 2) 闭包

在闭包中引入闭包外部的变量时，当闭包结束时此对象无法被垃圾回收（GC）。

    var a = function() {
      var largeStr = new Array(1000000).join('x');
      return function() {
        return largeStr;
      }
    }();

### 3) DOM泄露

当原有的COM被移除时，子结点引用没有被移除则无法回收。

    var select = document.querySelector;
    var treeRef = select('#tree');
    //在COM树中leafRef是treeFre的一个子结点
    var leafRef = select('#leaf');  
    var body = select('body');
    body.removeChild(treeRef);
    //#tree不能被回收入，因为treeRef还在
    //解决方法:
    treeRef = null;
    //tree还不能被回收，因为叶子结果leafRef还在
    leafRef = null;
    //现在#tree可以被释放了。

### 4) Timers计（定）时器泄露

定时器也是常见产生内存泄露的地方：

    for (var i = 0; i < 90000; i++) {
      var buggyObject = {
        callAgain: function() {
          var ref = this;
          var val = setTimeout(function() {
            ref.callAgain();
          }, 90000);
        }
      }
      buggyObject.callAgain();
      //虽然你想回收但是timer还在
      buggyObject = null;
    }
    
### 5) 调试内存

Chrome自带的内存调试工具可以很方便地查看内存使用情况和内存泄露：

在 Timeline -> Memory 点击record即可：
