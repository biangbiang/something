php $_POST 与 php://input的区别分析
==================================

$\_POST 与 php://input可以取到值，$HTTP\_RAW\_POST\_DATA 为空

$\_POST 以关联数组方式组织提交的数据，并对此进行编码处理，如urldecode，甚至编码转换

php://input 也可以实现此这个功能可以获得POST的原始数据。

**代码**

	echo   file_get_contents( "php://input ");

**实例**
 
	<form action="post.php" method="post"> 
	<input type="text" name="user"> 
	<input type="password" name="password"> 
	<input type="submit"> 
	</form>

**post.php**
 
	<? echo file_get_contents("php://input");?>

php://input 允许读取 POST 的原始数据。和 $HTTP\_RAW\_POST\_DATA 比起来，它给内存带来的压力较小，并且不需要任何特殊的 php.ini 设置。php://input 不能用于 enctype="multipart/form-data"。

**php $_POST**

$_POST 变量是一个数组，内容是由 HTTP POST 方法发送的变量名称和值。

$_POST 变量用于收集来自 method="post" 的表单中的值。从带有 POST 方法的表单发送的信息，对任何人都是不可见的（不会显示在浏览器的地址栏），并且对发送信息的量也没有限制。

**html**

	<form action="welcome.php" method="post">
	Enter your name: <input type="text" name="name" />
	Enter your age: <input type="text" name="age" />
	<input type="submit" />
	</form>

**welcome.php**

	Welcome <?php echo $_POST["name"]; ?>.<br />
	You are <?php echo $_POST["age"]; ?> years old!

通过 HTTP POST 发送的变量不会显示在 URL 中。 

变量没有长度限制
