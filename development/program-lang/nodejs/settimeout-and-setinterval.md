node.js 的定时器 setTimeout 和 setInterval
==========================================

node.js 下的定时器 

setTimeout(匿名函数,毫秒数) 表示多少毫秒后触发一次此匿名函数.

setInterval(匿名函数,毫秒数)  表示多少毫秒后重复触发此匿名函数.

了解javascript 的人都应该非常熟悉这两个函数,但是这些定时器却不是 ECMAScript 标准里所包含的函数,起码不是现在标准里的.

node.js 是运行的google V8环境下的服务器端javascript 脚本,当然V8是严格按照 EMCAScript 标准来实现的.

但是,V8 也实现了这些定时器函数 .

因为它们好用,功能强大,已经广泛被浏览器厂商默认支持.

可以说已经是深入人心.据说 ECMAScript 6 标准里要给它们一个名份.

所以,node.js 也实现了它们,使用是和客户端javascript 完全相同.

示例:

    var start = Date.now();
    console.log('开始行走江湖,当前时间:' + start);
    setTimeout(function () {
        console.log(Date.now() - start + '毫秒后,突然杀出一位好汉!\r\n');
    }, 2000);

结果:

![](http://biangbiangpic.b0.upaiyun.com/blog/157b007b280bc93bf08fb9de7c68040b.png)

有人会问,为啥设置2000毫秒杀出好汉,结果2002毫秒才杀出来?

尼玛,调用函数不需要时间吗?输入log不需要时间吗?再说了,你要杀就杀出来,那多没有面子!

setInterval 函数和 setTimeout 函数用法一致,产生的效果不一样,setInverval 函数是定时重复执行.

如果我不想让他重复执行了.或者说在2000毫秒内我突然退出江湖....不想半路再杀出一个好汉怎么办?

其实 setTimeout()  和 setInterval() 函数都返回一个定时器ID,你可以这样接收这个ID

    var oneID=setTimeout(......,2000);
    var twoID=setInverval(......,2000);

这2个ID的作用就是可以随时移除函数,用法如下:

    clearTimeout(oneID);
    clearInverval(twoID);
