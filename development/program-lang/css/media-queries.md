响应式web设计之CSS3 Media Queries
=================================

开始研究响应式web设计，CSS3 Media Queries是入门。

Media Queries，其作用就是允许添加表达式用以确定媒体的环境情况，以此来应用不同的样式表。换句话说，其允许我们在不改变内容的情况下，改变页面的布局以精确适应不同的设备。

那么，Media Queries是如何工作的？

**两种方式，一种是直接在link中判断设备的尺寸，然后引用不同的css文件：**

	<link rel="stylesheet" type="text/css" href="styleA.css" media="screen and (min-width: 400px)">

意思是当屏幕的宽度大于等于400px的时候，应用styleA.css

在media属性里：

* screen 是媒体类型里的一种，CSS2.1定义了10种媒体类型
* and 被称为关键字，其他关键字还包括 not(排除某种设备)，only(限定某种设备)
* (min-width: 400px) 就是媒体特性，其被放置在一对圆括号中。完整的特性参看 相关的Media features部分

	<link rel="stylesheet" type="text/css" href="styleB.css"  media="screen and (min-width: 600px) and (max-width: 800px)">

意思是当屏幕的宽度大于600小于800时，应用styleB.css

其它属性可看这里：http://www.swordair.com/blog/2010/08/431/

**另一种方式，即是直接写在<style>标签里：**

	@media screen and (max-width: 600px) { /*当屏幕尺寸小于600px时，应用下面的CSS样式*/
	  .class {
		background: #ccc;
	  }
	}

写法是前面加@media，其它跟link里的media属性相同

其实基本上就是样式覆盖~，判断设备，然后引用不同的样式文件覆盖。

要注意的是由于网页会根据屏幕宽度调整布局，所以不能使用绝对宽度的布局，也不能使用具有绝对宽度的元素。这一条非常重要，否则会出现横向滚动条。

 

### 补充：media query中的not only all等关键字

今天在群里一群友问起 @media only screen and (min-width: 320px) 中only是什么意思，查了些资料。

not: not是用来排除掉某些特定的设备的，比如 @media not print（非打印设备）、

only: 用来定某种特别的媒体类型。对于支持Media Queries的移动设备来说，如果存在only关键字，移动设备的Web浏览器会忽略only关键字并直接根据后面的表达式应用样式文件。对于不支持Media Queries的设备但能够读取Media Type类型的Web浏览器，遇到only关键字时会忽略这个样式文件。

all: 所有设备，这个应该经常看到

还有其它一些：

![](http://biangbiangpic.b0.upaiyun.com/blog/3c1ae09ea140119d718d58f7a1da82a8.png)

相关资料扩展：http://book.51cto.com/art/201204/328362.htm

　　　　　　  http://www.w3cplus.com/content/css3-media-queries

　　　　　　  http://www.w3.org/TR/CSS2/media.html#media-types

----------------------------------华丽的分割线-----------------------------------------------------------

以下是demo

一个三栏布局的，在不同的尺寸下，变为两栏，再变为一栏~

![](http://biangbiangpic.b0.upaiyun.com/blog/e4d24219fa24947e7629e02a15fc304f.jpg)

代码：

	<!DOCTYPE HTML>
	<html>
	<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1" />
	<title>css3-media-queries-demo</title>
	<style>
	body, div, dl, dt, dd, ul, ol, li, h1, h2, h3, h4, h5, h6, pre, form, fieldset, input, textarea, p, blockquote, th, td {
		padding: 0;
		margin: 0;
	}
	.content{
		zoom:1;
	}
	.content:after{
		content: ".";
		display: block;
		height: 0;
		clear: both;
		visibility: hidden; 
	}
	.leftBox, .rightBox{
		float: left;
		width: 20%;
		height: 500px;
		margin: 5px;
		background: #ffccf7;
		display: inline;
		-webkit-transition: width 1s ease;
		-moz-transition: width 1s ease;
		-o-transition: width 1s ease;
		-ms-transition: width 2s ease;
		transition: width 1s ease;
	}
	.middleBox{
		float: left;
		width: 50%;
		height: 800px;
		margin: 5px;
		background: #b1fffc;
		display: inline;
		-webkit-transition: width 1s ease;
		-moz-transition: width 1s ease;
		-o-transition: width 1s ease;
		-ms-transition: width 1s ease;
		transition: width 1s ease;
	}
	.rightBox{
		background: #fffab1;
	}
	@media only screen and (min-width: 1024px){
		.content{
				width: 1000px;
				margin: auto
			}
	}
	@media only screen and (min-width: 400px) and (max-width: 1024px){
		.rightBox{
			width: 0;
		}
		.leftBox{ width: 30%}
		.middleBox{ width: 65%}
	}
	@media only screen and (max-width: 400px){
		.leftBox, .rightBox, .middleBox{ 
			width: 98%;
			height: 200px;
		}
	}
	</style>
	</head>

	<body>
	<div class="content">
	  <div class="leftBox"></div>
	  <div class="middleBox"></div>
	  <div class="rightBox"></div>
	</div>
	</body>
	</html>
