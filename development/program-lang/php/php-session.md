关于 PHP 中 Session 的几个问题
==============================

### 什么是 Session

在 web 应用开发中，Session 被称为会话。主要被用于保存某个访问者的数据。

由于 HTTP 无状态的特点，服务端是不会记住客户端的，对服务端来说，每一个请求都是全新的。

既然如此，那么服务端怎么知道是哪个访问者在请求它呢？又如何将不同的数据对应上正确的访问者？答案是，给访问者一个唯一获取 Session 中数据的身份标示。

打个比方：当我们去超市购物时，被保安告之我们是不能带物品进去的，必须将物品寄放在超市的储物箱中。我们把物品交给了他，他怎么知道这些物品谁是谁的，于是他给了我们不同的钥匙。当我们要取走我们的物品时，用唯一的钥匙打开对应的箱子即可。

就如同上面的比方一样，可以将 Session 理解为存放我们数据的“箱子”，当然，这些“箱子”都在服务端那。服务器给访问者唯一的“钥匙”，这个“钥匙”被称作 session\_id。访问者凭借自己的 session\_id，就能获取到自己存在服务器端的数据。

session\_id 通过两种方式传给访问者（客户端）：URL 或 cookie。详情参见：[传送会话ID](http://php.net/manual/zh/session.idpassing.php)

### Session 和 Cookie 有什么关系

Cookie 也是由于 HTTP 无状态的特点而产生的技术。也被用于保存访问者的身份标示和一些数据。每次客户端发起 HTTP 请求时，会将 Cookie 数据加到 HTTP header 中，提交给服务端。这样服务端就可以根据 Cookie 的内容知道访问者的信息了。

可以说，Session 和 Cookie 做着相似的事情，只是 Session 是将数据保存在服务端，通过客户端提交来的 session\_id 来获取对应的数据；而 Cookie 是将数据保存在客户端，每次发起请求时将数据提交给服务端的。

上面提到，session\_id 可以通过 URL 或 cookie 来传递，由于 URL 的方式比 cookie 的方式更加不安全且使用不方便，所以一般是采用 cookie 来传递 session\_id。

服务端生成 session\_id，通过 HTTP 报文发送给客户端（比如浏览器），客户端收到后按指示创建保存着 session\_id 的 cookie。cookie 是以 key/value 形式保存的，看上去大概就这个样子的：PHPSESSID=e4tqo2ajfbqqia9prm8t83b1f2。在 PHP 中，保存 session\_id 的 cookie 名称默认叫作 PHPSESSID，这个名称可以通过 php.ini 中 session.name 来修改，也可以通过函数 session\_name() 来修改。

### 为什么不推荐使用 PHP 自带的 files 型 Session 处理器

在 PHP 中，默认的 Session 处理器是 files，处理器可以用户自己实现（参见：自定义会话管理器）。我知道的成熟的 Session 处理器还有很多：Redis、Memcached、MongoDB……

为什么不推荐使用 PHP 自带的 files 类型处理器，PHP 官方手册中给出过这样一段 Note：

无论是通过调用函数 session\_start() 手动开启会话， 还是使用配置项 session.auto\_start 自动开启会话， 对于基于文件的会话数据保存（PHP 的默认行为）而言， 在会话开始的时候都会给会话数据文件加锁， 直到 PHP 脚本执行完毕或者显式调用 session\_write\_close() 来保存会话数据。 在此期间，其他脚本不可以访问同一个会话数据文件。

上述引用参见：[Session 的基本用法](http://php.net/manual/zh/session.examples.basic.php)

为了证明这段话，我们创建一下 2 个文件：

文件：session1.php

    <?php
    session_start();

    sleep(5);

    var_dump($_SESSION);
    ?>

文件：session2.php

    <?php
    session_start();

    var_dump($_SESSION);
    ?>

在同一个浏览器中，先访问 http://127.0.0.1/session1.php，然后在当前浏览器新的标签页立刻访问 http://127.0.0.1/session2.php。实验发现，session1.php 等了 5 秒钟才有输出，而 session2.php 也等到了将近 5 秒才有输出。而单独访问 session2.php 是秒开的。在一个浏览器中访问 session1.php，然后立刻在另外一个浏览器中访问 session2.php。结果是 session1.php 等待 5 秒钟有输出，而 session2.php 是秒开的。

分析一下造成这个现象的原因：上面例子中，默认使用 Cookie 来传递 session\_id，而且 Cookie 的作用域是相同。这样，在同一个浏览器中访问这 2 个地址，提交给服务器的 session\_id 就是相同的（这样才能标记访问者，这是我们期望的效果）。当访问 session1.php 时，PHP 根据提交的 session\_id，在服务器保存 Session 文件的路径（默认为 /tmp，通过 php.ini 中的 session.save\_path 或者函数 session\_save\_path() 来修改）中找到了对应的 Session 文件，并对其加锁。如果不显式调用 session\_write\_close()，那么直到当前 PHP 脚本执行完毕才会释放文件锁。如果在脚本中有比较耗时的操作（比如例子中的 sleep(5)），那么另一个持有相同 session\_id 的请求由于文件被锁，所以只能被迫等待，于是就发生了请求阻塞的情况。

既然如此，在使用完 Session 后，立刻显示调用 session\_write\_close() 是不是就解决问题了哩？比如上面例子中，在 sleep(5) 前面调用 session\_write\_close()。
确实，这样 session2.php 就不会被 session1.php 所阻塞。但是，显示调用了 session\_write\_close() 就意味着将数据写到文件中并结束当前会话。那么，在后面代码中要使用 Session 时，必须重新调用 session\_start()。

例如：

    <?php
    session_start();
    $_SESSION['name'] = 'Jing';
    var_dump($_SESSION);
    session_write_close();

    sleep(5);

    session_start();
    $_SESSION['name'] = 'Mr.Jing';
    var_dump($_SESSION);
    ?>

官方给出的方案：

对于大量使用 Ajax 或者并发请求的网站而言，这可能是一个严重的问题。 解决这个问题最简单的做法是如果修改了会话中的变量， 那么应该尽快调用 session\_write\_close() 来保存会话数据并释放文件锁。 还有一种选择就是使用支持并发操作的会话保存管理器来替代文件会话保存管理器。

我推荐的方式是使用 Redis 作为 Session 的处理器。

拓展阅读：

[为什么不能用 memcached 存储 Session](http://www.infoq.com/cn/news/2015/01/memcached-store-session)

[如何使用 Redis 作为 PHP Session handler](https://github.com/phpredis/phpredis#php-session-handler)

### Session 数据是什么时候被删除的

这是一道经常被面试官问起的问题。

先看看官方手册中的说明：

session.gc\_maxlifetime 指定过了多少秒之后数据就会被视为"垃圾"并被清除。 垃圾搜集可能会在 session 启动的时候开始（ 取决于 session.gc\_probability 和 session.gc\_divisor）。 session.gc\_probability 与 session.gc\_divisor 合起来用来管理 gc（garbage collection 垃圾回收）进程启动的概率。此概率用 gc\_probability/gc\_divisor 计算得来。例如 1/100 意味着在每个请求中有 1% 的概率启动 gc 进程。session.gc\_probability 默认为 1，session.gc\_divisor 默认为 100。

继续用我上面那个不太恰当的比方吧：如果我们把物品放在超市的储物箱中而不取走，过了很久（比如一个月），那么保安就要清理这些储物箱中的物品了。当然并不是超过期限了保安就一定会来清理，也许他懒，又或者他压根就没有想起来这件事情。

再看看两段手册的引用：

如果使用默认的基于文件的会话处理器，则文件系统必须保持跟踪访问时间（atime）。Windows FAT 文件系统不行，因此如果必须使用 FAT 文件系统或者其他不能跟踪 atime 的文件系统，那就不得不想别的办法来处理会话数据的垃圾回收。自 PHP 4.2.3 起用 mtime（修改时间）来代替了 atime。因此对于不能跟踪 atime 的文件系统也没问题了。

GC 的运行时机并不是精准的，带有一定的或然性，所以这个设置项并不能确保旧的会话数据被删除。某些会话存储处理模块不使用此设置项。

对于这种删除机制，我是存疑的。

比如 gc\_probability/gc\_divisor 设置得比较大，或者网站的请求量比较大，那么 GC 进程启动就会比较频繁。

还有，GC 进程启动后都需要遍历 Session 文件列表，对比文件的修改时间和服务端的当前时间，判断文件是否过期而决定是否删除文件。

这也是我觉得不应该使用 PHP 自带的 files 型 Session 处理器的原因。而 Redis 或 Memcached 天生就支持 key/value 过期机制的，用于作为会话处理器很合适。或者自己实现一个基于文件的处理器，当根据 session\_id 获取对应的单个 Session 文件时判断文件是否过期。

### 为什么重启浏览器后 Session 数据就取不到了

session.cookie\_lifetime 以秒数指定了发送到浏览器的 cookie 的生命周期。值为 0 表示"直到关闭浏览器"。默认为 0。

其实，并不是 Session 数据被删除（也有可能是，概率比较小，参见上一节）。只是关闭浏览器时，保存 session\_id 的 Cookie 没有了。也就是你弄丢了打开超市储物箱的钥匙（session\_id）。

同理，浏览器 Cookie 被手动清除或者其他软件清除也会造成这个结果。

### 为什么浏览器开着，我很久没有操作就被登出了

这个是称为“防呆”，为了保护用户账户安全的。

这个小节放进来，是因为这个功能的实现可能和 Session 的删除机制有关（之所以说是可能，是因为这个功能不一定要借住 Session 实现，用 Cookie 也同样可以实现）。

说简单一点，就是长时间没有操作，服务端的 Session 文件过期被删除了。

### 一个有意思的事情

在我试验的过程中，发现了小有意思的事情：我把 GC 启动的概率设置为 100%。如果只有一个访问者请求，该访问者即使过了很久（超过了过期时间）后才发起第二次请求，那么 Session 数据也还是存在的（'session.save\_path' 目录下面的 Session 文件存在）。是的，明明就超过了过期时间，却没有被 GC 删除。这时，我用另外一个浏览器访问时（相对于另一个访问者），这次请求生成了新的 Session 文件，而上一个浏览器请求生成的那个 Session 文件终于没有了（之前那个 Session 文件在 'session.save\_path' 目录下面的消失了）。

还有，发现 Session 文件被删除后，再次请求，还是会生成和之前文件名相同的 Session 文件（因为浏览器并没有关闭，再次请求发送的 session\_id 是相同的，所以重新生成的 Session 文件的文件名还是一样的）。但是，我不理解的是：这个重新出现的文件的创建时间竟然是第一次的那个创建时间，难道它是从回收站中回来的？（确实，我做这个试验时是在 window 下进行的）

我猜测的原因是这样：当启动会话后，PHP 根据 session\_id 找到并打开了对应的 Session 文件，然后才启动 GC 进程。GC 进程就只检查除了当前这个 Session 文件外的其他文件，发现过期的就干掉。所有，即使当前这个 Session 文件已经过期了，GC 也没有删除它。

我认为这个不合理的。

由于发生这种情况影响也不大（毕竟线上请求很多，当前请求的过期文件被其他请求唤起的 GC 干掉的可能性是比较大的） + 我没有信心去看 PHP 源代码 + 我并不在线上使用 PHP 自带的 files 型 Session 处理器。所以，这个问题我就没有深入研究了。请谅解。

    <?php
    // 过期时间设置为 30 秒
    ini_set('session.gc_maxlifetime', '30');

    // GC 启动概率设置为 100%
    ini_set('session.gc_probability', '100');
    ini_set('session.gc_divisor', '100');

    session_start();
    $_SESSION['name'] = 'Jing';

    var_dump($_SESSION);
    ?>
