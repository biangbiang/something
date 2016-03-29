js中Math.random()生成指定范围数值的随机数
=========================================

Math.random函数就不像php的rand函数一样可以生成指数范围的数据了，math.random只是生成了一个伪随机数，之后还要经过我们处理才行哦。

今天又有网友问到我 JavaScript 怎么生成指定范围数值随机数。Math.random() 这个方法相信大家都知道，是用来生成随机数的。不过一般的参考手册时却没有说明如何用这个方法来生成指定范围内的随机数。这次我就来详细的介绍一下Math.random()，以及如何用它来生成制定范围内的随机数。

### w3school的random()教程

**定义和用法**

random() 方法可返回介于 0 ~ 1 之间的一个随机数。

**语法**

Math.random()

**返回值**

0.0 ~ 1.0 之间的一个伪随机数。

**实例**

在本例中，我们将取得介于 0 到 1 之间的一个随机数：

    <script type="text/javascript">
    document.write(Math.random());
    </script>
    // 输出：
    0.15246391076246546

### 如何生成指定范围值的随机数

看完w3school的教程，应该知道Math.random()方法的基本用法了。

利用 parseInt()、Math.floor() 或者 Math.ceil()进行四舍五入处理

我们看到，直接使用Math.random()方法，生成的是一个小于1的数，所以：

    Math.random()*5

得到的结果是一个小于5的随机数。而我们通常希望得到的是0-5之间的整数，所以我们需要对得到的结果四舍五入处理一下，从而得到我们期望的整数。parseInt()、Math.floor()和Math.ceil()都可以起到四舍五入的作用。

    var randomNum = Math.random()*5;
    alert(randomNum); // 2.9045290905811183 
    alert(parseInt(randomNum,10)); // 2
    alert(Math.floor(randomNum)); // 2
    alert(Math.ceil(randomNum)); // 3

由测试的代码我们可以看到，parseInt()和Math.floor()的效果是一样的，都是向下取整数部分。所以parseInt(Math.random()*5,10)和Math.floor(Math.random()*5)都是生成的0-4之间的随机数，Math.ceil(Math.random()*5)则是生成的1-5之间的随机数。

### 生成指定范围数值随机数

所以，如果你希望生成1到任意值的随机数，公式就是这样的：

    // max - 期望的最大值
    parseInt(Math.random()*max,10)+1;
    Math.floor(Math.random()*max)+1;
    Math.ceil(Math.random()*max);

如果你希望生成0到任意值的随机数，公式就是这样的：

    // max - 期望的最大值
    parseInt(Math.random()*(max+1),10);
    Math.floor(Math.random()*(max+1));

如果你希望生成任意值到任意值的随机数，公式就是这样的：

    // max - 期望的最大值
    // min - 期望的最小值 
    parseInt(Math.random()*(max-min+1)+min,10);
    Math.floor(Math.random()*(max-min+1)+min);
