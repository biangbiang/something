开发人员使数据库面临风险的十大方面
==================================

有些最重要的数据库保护方法是从接触敏感数据存储的开发人员开始的。当今的WEB应用程序依赖于企业的最敏感的数据存储，用以保持订单系统的运行，保持合伙人之间的合作，保证无论互联网用户身处何处都能够接触重要的业务信息。

虽然这种对关键数据的简易访问已经极大地提高了工作人员的效率，并提高了顾客的购买欲，但它也为关键数据库打开了巨大的风险之门。不幸的是，许多风险是由缺乏资源的开发人员带来的，他们往往无法得到足够的时间、金钱、教育以及来自管理人员的支持，因而无法设计开发没有漏洞的应用程序。

在上述资源无法到位时，开发人员往往会犯这些错误：

### 1、过于相信输入方法

当今开发人员将数据库置于风险之中的一种重要方法是，使其应用程序遭受SQL注入攻击。当开发人员过于相信用户输入时，就往往会出现SQL注入漏洞。

如果不对其进行验证，例如，如果不验证一个只接受数字的电话号码字段，开发者就有可能允许黑客对数据库进行终端访问。例如，插入到表单域中的单引号有可能过早地关闭应用程序本应实施的合法SQL查询，从而使攻击者可以构建并提交新的查询。

解决这种问题的最好方法之一就是使用参数化查询。

参数化查询可以防止攻击者改变查询中的逻辑或代码，并阻止大多数SQL注入攻击。参数化查询出现的时间就像数据库一样长，它不像安全的其它许多领域，使用参数化查询也是保障性能和代码可维护性的最佳方法。

但开发人员不应当停留于此。还要对用户输入进行净化和验证，因为有时参数化并不可行。即使可行，攻击者也可以将非验证的输入用于其它恶意目的，如跨站脚本攻击等。

最老套且最糟糕的问题是，不对用户输入进行净化，即在对数据库运行查询之前，不剔除用户输入中并不需要的字符，例如不移除撇号或分号。

### 2、数据库错误消息显示给终端用户

在应用程序的SQL查询出现问题时，如果开发人员允许弹出特定的错误消息，这也许有助于诊断，但这也向攻击者提供了一个探查后端数据库的内部工作机制的很好途径。

通过得到的数据库错误消息，攻击者就会了解数据库的组织结构和应用程序查询等许多重要信息，从而更容易展开攻击。

错误消息可以泄露关于应用程序连接的数据库类型、底层设计等的线索。

这种错误会为盲目SQL注入攻击打开大门，所以开发者应当在页面显示一般性的错误消息而非特定消息。

### 3、轻率地对待口令

为了图方便，许多开发者用多种不安全的方法来轻率地对待用户口令。例如，他们可能将口令硬编码到应用程序中。

最明显的数据库风险之一在于硬编码口令和存在于配置文件中的口令。这两种设计选择都依赖于这种脆弱的安全性，他们假定攻击者不会访问这种信息，因而开发者并不关心文件或其中信息的安全性。

同样危险的还有将口令存放到纯文本中的方法，其中包括不对用户输入的口令进行哈希，以及并不对哈希加盐（加盐：将随机位添加到哈希中）。

此外，还有不健全的口令管理，没有将强健的口令认证构建到应用程序中。

### 4、使所有的连接都是“超级”的

同样的，许多开发者通过“根”或其它一些超级用户账户来将应用程序连接到数据库中，从而将数据库置于风险之中。这通常是一个与硬编码口令的应用程序有关的问题，这是因为在这些情况下很难实施适当的特权管理。

普通的应用操作很少需要由“超级”特权所许可的访问，允许应用程序使用超级特权用户，会为不适当的非授权活动创造机会。所有的应用程序都应当使用最少特权的用户凭证连接到数据库。

### 5、相信存储过程是SQL注入的解决之道

当今的许多开发人员相信，存储过程是防止SQL注入的一种可靠方法。

事实上，如果存储过程自身的代码中包含漏洞，或者如果存储过程被以一种不安全的方式调用，它就并不能防止SQL注入。

在存储过程中可以发生串的连接或并置。即使存储过程的调用是准备好的语句，如果在存储过程自身内部构建和执行查询，它也有可能易遭受攻击。

### 6、将调试代码留放到生产环境中

正如厨师在做完了美味佳肴之后要打扫厨房一样，开发人员必须在将程序投放到生产环境中之前，必须清理其代码，以免打开数据库的后门。导致这种后门最常见的且易被遗忘的要素是遗留到生产环境中调试代码。

开发人员将后门放到程序中，其目的是为了调试，从而可以直接启用数据库的查询。忘记在生产环境中清除这种调试代码是愚蠢的，但也是很常见的错误。

### 7、劣质加密

比不使用加密更为糟糕的唯一问题是，错误地使用加密，因为这这种加密给企业一种虚假的安全感。

问题存在于细节中，不管是一个哈希函数或是一个加密例程。如果你并不知道如何正确地实施加密，那就将任务委托给一位专家，并且不要过早地将应用程序投放到生产环境中。

开发人员应当谨慎地对待其加密技术方面的技能和技巧。当今的黑客喜欢本地的加密设计，因为这种设计通常很差劲。

本地的加密几乎不能给有经验的攻击者提供什么价值，并且会给企业带来一种虚假的安全感。

### 8、盲目相信第三方代码

使用第三方的代码可能为开发人员节约大量的时间，但是开发人员不能在测试代码问题上抄近路，以确保所复制的代码不会给应用程序带来易受攻击的代码。

开发人员需要理解，自己要为整个应用程序的威胁分析负责，而不仅仅为自己编写的代码负责。

同样地，如果开发者希望限制那些给数据库带来风险的漏洞数量，他们就应当使用最新的开发框架。

许多开发人员会使用WordPress、 Joomla或其它类型的应用程序框架，却没有用最新的安全补丁保持其最新。许多类似的电脑都易遭受攻击，应用程序也如此。

### 9、轻率地实施REST(表述性状态转移)架构

表述性状态转移（REST）架构可能是一个有用的范例，但太多的工具会直接从数据库生成REST接口，从而将接口与数据库的设计架构联结起来，并可以暴露攻击者能够利用的信息。开发人员应当为一系列抽象的应用程序的特定资源类型和资源的适当操作设计REST接口，而不是把接口直接设计到物理的数据表和非特殊操作。

### 10、随处乱放备份的数据库副本

许多开发人员常常被责怪没有在测试环境（在其中，开发者寻求测试其应用程序的可选方式）中使用动态数据。不幸的是，这些编码者的许多人会转向备份数据库。

网站可以很好地保护动态数据库，但是否同样保护好了自己的数据库备份呢？在备份中，一周前的数据可能和动态数据一样具有破坏力，开发者可能会使用备份来针对生产性数据测试其工作，所以开发环境必须小心地遵循生产环境中的安全标准。安全策略应当为数据的所有副本负责，而不仅仅是在线副本。
