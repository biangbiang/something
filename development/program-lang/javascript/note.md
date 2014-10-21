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

### window.location()

window.location 对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。

### history

1. back()：加载 history 列表中的前一个 URL
2. forward()：加载 history 列表中的下一个 URL
3. go()：加载 history 列表中的某个具体页面

### 死链接

javascript:void(0) 

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

### 变量

在javascript中，没有使用var声明的变量都被当成全局变量来处理了

### buckaroo

buckaroo里面特殊封装runtime.js Date.format('Y-m-d H:i:s')
