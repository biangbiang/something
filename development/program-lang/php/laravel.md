Laravel
=======

## 简介

Laravel是一套简洁、优雅的PHP Web开发框架(PHP Web Framework)。它可以让你从面条一样杂乱的代码中解脱出来；它可以帮你构建一个完美的网络APP，而且每行代码都可以简洁、富于表达力。

---

## 功能特点

1. 语法更富有表现力

  你知道下面这行代码里 “true” 代表什么意思么?

	    $uri = Uri::create(‘some/uri’, array(), array(), true);

  另外，你知道其他参数在这里的意思么（除了第一个）？当然你不知道。因为这行代码没有表现力。

  再看看这段代码:

	    $url = URL::to_secure(‘some/uri’);

  这个表达式使用HTTPS协议创建了一条URL链接， 事实上，上面两种写法都在做同样的事情，但哪一个更一目了然，更富有表现力呢？

2. 高质量的文档

  CodeIgniter非常流行原因之一是它有良好的文档。这对程序员来说是十分方便的。相比之下，Kohana一个在技术上比CI更加优秀的框架，但你猜怎么着？ 大家不在乎Kohana技术有多强，因为Kohana的文档实在是太糟了。　而Laravel 有一个非常棒的的社区支持。Laravel代码本身的表现力和良好的文档使PHP程序编写令人愉快。

3. 丰富的扩展包

  Bundle是Laravel中对扩展包的称呼。它可以是任何东西 -- 大到完整的ORM，小到除错(debug)工具，仅仅复制&粘贴就能安装任何扩展包！Laravel的扩展包由世界各地的开发者贡献，而且还在不断增加中。

4. 开源、托管在GITHUB上

  Laravel是完全开源的。所有代码都可以从Github上获取，并且欢迎你贡献出自己的力量。

---

## 技术特点

1. Bundle是Laravel的扩展包组织形式或称呼。Laravel的扩展包仓库已经相当成熟了，可以很容易的帮你把扩展包（bundle）安装到你的应用中。你可以选择下载一个扩展包（bundle）然后拷贝到bundles目录，或者通过命令行工具“Artisan”自动安装。

2. 在Laravel中已经具有了一套高级的PHP ActiveRecord实现 -- Eloquent ORM。它能方便的将“约束（constraints）”应用到关系的双方，这样你就具有了对数据的完全控制，而且享受到ActiveRecord的所有便利。Eloquent原生支持Fluent中查询构造器（query-builder）的所有方法。

3. 应用逻辑（Application Logic）可以在控制器（controllers）中实现，也可以直接集成到路由（route）声明中，并且语法和Sinatra框架类似。Laravel的设计理念是：给开发者以最大的灵活性，既能创建非常小的网站也能构建大型的企业应用。

4. 反向路由（Reverse Routing）赋予你通过路由（routes）名称创建链接（URI)的能力。只需使用路由名称（route name），Laravel就会自动帮你创建正确的URI。这样你就可以随时改变你的路由（routes），Laravel会帮你自动更新所有相关的链接。

5. Restful控制器（Restful Controllers）是一项区分GET和POST请求逻辑的可选方式。比如在一个用户登陆逻辑中，你声明了一个`get_login()`的动作（action）来处理获取登陆页面的服务；同时也声明了一个`post_login()`动作（action）来校验表单POST过来的数据，并且在验证之后，做出重新转向（redirect）到登陆页面还是转向控制台的决定。

6. 自动加载类（Class Auto-loading）简化了类（class）的加载工作，以后就可以不用去维护自动加载配置表和非必须的组件加载工作了。当你想加载任何库（library）或模型（model）时，立即使用就行了，Laravel会自动帮你加载需要的文件。

7. 视图组装器（View Composers）本质上就是一段代码，这段代码在视图（View）加载时会自动执行。最好的例子就是博客中的侧边随机文章推荐，“视图组装器”中包含了加载随机文章推荐的逻辑，这样，你只需要加载内容区域的视图（view）就行了，其它的事情Laravel会帮你自动完成。

8. 反向控制容器（IoC container）提供了生成新对象、随时实例化对象、访问单例（singleton）对象的便捷方式。反向控制（IoC）意味着你几乎不需要特意去加载外部的库（libraries），就可以在代码中的任意位置访问这些对象，并且不需要忍受繁杂、冗余的代码结构。

9. 迁移（Migrations）就像是版本控制（version control）工具，不过，它管理的是数据库范式，并且直接集成在了Laravel中。你可以使用“Artisan”命令行工具生成、执行“迁移”指令。当你的小组成员改变了数据库范式的时候，你就可以轻松的通过版本控制工具更新当前工程，然后执行“迁移"指令即可，好了，你的数据库已经是最新的了！

10. 单元测试（Unit-Testing）是Laravel中很重要的部分。Laravel自身就包含数以百计的测试用例，以保障任何一处的修改不会影响其它部分的功能，这就是为什么在业内Laravel被认为是最稳版本的原因之一。Laravel也提供了方便的功能，让你自己的代码容易的进行单元测试。通过Artisan命令行工具就可以运行所有的测试用例。

11. 自动分页（Automatic Pagination）功能避免了在你的业务逻辑中混入大量无关分页配置代码。方便的是不需要记住当前页，只要从数据库中获取总的条目数量，然后使用limit/offset获取选定的数据，最后调用‘paginate’方法，让Laravel将各页链接输出到指定的视图（View)中即可，Laravel会替你自动完成所有工作。Laravel的自动分页系统被设计为容易实现、易于修改。虽然Laravel可以自动处理这些工作，但是不要忘了调用相应方法和手动配置分页系统哦！
