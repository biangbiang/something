javascript笔记
=============

### window.open(URL,name,features,replace)

* URL：一个可选的字符串，声明了要在新窗口中显示的文档的 URL。如果省略了这个参数，或者它的值是空字符串，那么新窗口就不会显示任何文档。

* name：一个可选的字符串，该字符串是一个由逗号分隔的特征列表，其中包括数字、字母和下划线，该字符声明了新窗口的名称。这个名称可以用作标记 `<a>` 和 `<form>` 的属性 target 的值。如果该参数指定了一个已经存在的窗口，那么 open() 方法就不再创建一个新窗口，而只是返回对指定窗口的引用。在这种情况下，features 将被忽略。

* features：一个可选的字符串，声明了新窗口要显示的标准浏览器的特征。如果省略该参数，新窗口将具有所有标准特征。在窗口特征这个表格中，我们对该字符串的格式进行了详细的说明。

* replace：一个可选的布尔值。规定了装载到窗口的 URL 是在窗口的浏览历史中创建一个新条目，还是替换浏览历史中的当前条目。支持下面的值：

>true - URL 替换浏览历史中的当前条目。

>false - URL 在浏览历史中创建新的条目。

在当前页面打开：window.open(url, '_self')

---

### window.location()

window.location 对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。

---

### history

1. back()：加载 history 列表中的前一个 URL
2. forward()：加载 history 列表中的下一个 URL
3. go()：加载 history 列表中的某个具体页面

---

### 死链接

javascript:void(0) 

---

### json

JSON.stringify：把对象类型转化成字符串类型

JSON.parse：把字符串转化成json对象

parse用于从一个字符串中解析出json对象,如

	var str = '{"name":"huangxiaojian","age":"23"}'

结果：

	JSON.parse(str)
	Object
	age: "23"
	name: "huangxiaojian"
	__proto__: Object

注意：单引号写在{}外，每个属性名都必须用双引号，否则会抛出异常。

stringify()用于从一个对象解析出字符串，如

	var a = {a:1,b:2}

结果：

	JSON.stringify(a)
	"{"a":1,"b":2}"

---

### 变量

在javascript中，没有使用var声明的变量都被当成全局变量来处理了

---

### 为什么JavaScript里面typeof(null)的值是"object"？

#### 回答一

1. null不是一个空引用, 而是一个原始值, 参考ECMAScript5.1中文版 4.3.11节; 它只是期望此处将引用一个对象, 注意是"期望", 参考 null - JavaScript.

2. typeof null结果是object, 这是个历史遗留bug, 参考 typeof - JavaScript

3. 在ECMA6中, 曾经有提案为历史平凡, 将type null的值纠正为null, 但最后提案被拒了. 理由是历史遗留代码太多, 不想得罪人, 不如继续将错就错当和事老, 参考 harmony:typeof_null [ES Wiki]

#### 回答二

1. 娘胎里带出来的.

  JS类型值是存在32 BIT 单元里,32位有1-3位表示TYPE TAG,其它位表示真实值
  
  而表示object的标记位正好是低三位都是0
  
  000: object. The data is a reference to an object.
  
  而js 里的Null 是机器码NULL空指针, (0x00 is most platforms).所以空指针引用 加上 对象标记还是0,最终体现的类型还是object..
  
  这也就是为什么Number(null)===0吧...
  
  The history of “typeof null”

2. 曾经有提案 typeof null === 'null'.但提案被拒绝

  harmony:typeof_null [ES Wiki]

---

### js如何判断一个对象是不是Array

本来判断一个对象类型用typeof是最好的，不过对于Array类型是不适用的

可以使用 instanceof操作符

    var arrayStr=new Array("1","2","3","4","5");    
    alert(arrayStr instanceof Array);    

当然以上在一个简单的页面布局里面是没有问题的，如果是复杂页面情况，入获取的是frame内部的Array对象，可以用这个函数判断：

    function isArray(obj) {      
          return Object.prototype.toString.call(obj) === '[object Array]';       
    }
