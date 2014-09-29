DIV+CSS两种盒子模型
==================

利用CSS来布局页面布局DIV有点逻辑性!

重点理解盒子模型,标准流和非标准流的区别,还有定位原理!把这3个攻破了,就非常简单了!多实践多参考!

最后就是兼容问题了,在实践中自然就有经验了!这些兼容技巧都是经验来的!

---

盒子模型有两种，分别是 IE 盒子模型和标准 W3C 盒子模型。他们对盒子模型的解释各不相同，

先来看看我们熟悉的标准盒子模型：

![标准盒子模型](http://biangbiangpic.b0.upaiyun.com/blog/685f79ed1376671b5e5ed80d980fee50.jpg)

从上图可以看到标准 W3C 盒子模型的范围包括 margin、border、padding、content，并且 content 部分不包含其他部分。　

![IE盒子模型](http://biangbiangpic.b0.upaiyun.com/blog/675ea88adc86a7d6244e232c9e6cdc2e.jpg)

从上图可以看到 IE 盒子模型的范围也包括 margin、border、padding、content，和标准 W3C 盒子模型不同的是：IE 盒子模型的 content 部分包含了 border 和 pading。

---

例：一个盒子的 margin 为 20px，border 为 1px，padding 为 10px，content 的宽为 200px、高为 50px，如果用标准 W3C 盒子模型解释，那么这个盒子需要占据的位置为：宽 `20*2+1*2+10*2+200=262px`、高 `20*2+1*2*10*2+50=112px`，盒子的实际大小为：宽 `1*2+10*2+200=222px`、高 `1*2+10*2+50=72px`；如果用IE 盒子模型，那么这个盒子需要占据的位置为：宽 `20*2+200=240px`、高 `20*2+50=70px`，盒子的实际大小为：宽 200px、高 50px。

那应该选择哪中盒子模型呢？当然是“标准 W3C 盒子模型”了。怎么样才算是选择了“标准 W3C 盒子模型”呢？很简单，就是在网页的顶部加上 DOCTYPE 声明。如果不加 DOCTYPE 声明，那么各个浏览器会根据自己的行为去理解网页，即 IE 浏览器会采用 IE 盒子模型去解释你的盒子，而 FF 会采用标准 W3C 盒子模型解释你的盒子，所以网页在不同的浏览器中就显示的不一样了。反之，如果加上了 DOCTYPE 声明，那么所有浏览器都会采用标准 W3C 盒子模型去解释你的盒子，网页就能在各个浏览器中显示一致了。

---

再用 jQuery 做的例子来证实一下。

代码1：

	<html>
	<head>
	<title>你用的盒子模型是？</title>
	<script language="javascript" src="jquery.min.js"></script>
	<script language="javascript">
	var sBox = $.boxModel ? "标准W3C":"IE";
	document.write("您的页面目前支持："+sBox+"盒子模型");
	</script>
	</head>
	<body>
	</body>
	</html>

上面的代码没有加上 DOCTYPE 声明，在 IE 浏览器中显示“IE盒子模型”，在 FF 浏览器中显示“标准 W3C 盒子模型”。

代码2：

	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
	<html>
	<head>
	<title>你用的盒子模型是标准W3C盒子模型</title>
	<script language="javascript" src="jquery.min.js"></script>
	<script language="javascript">
	var sBox = $.boxModel ? "标准W3C":"IE";
	document.write("您的页面目前支持："+sBox+"盒子模型");
	</script>
	</head>
	<body>
	</body>
	</html>

代码2 与代码1 唯一的不同的就是顶部加了 DOCTYPE 声明。在所有浏览器中都显示“标准 W3C 盒子模型”。

所以为了让网页能兼容各个浏览器，让我们用标准 W3C 盒子模型 
