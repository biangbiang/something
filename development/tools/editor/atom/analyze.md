产品解析：Github Atom
===================

### Atom是什么

Atom是github内部的编辑软件，据说已经使用了6年之久。按照atom的博客：

	Our goal is a zero-compromise combination of hackability and usability: an editor that will be welcoming to an elementary school student on their first day learning to code, but also a tool they won't outgrow as they develop into seasoned hackers.

按照atom作者的意思：textmate/sublime text提供了受限的扩展性；而vim/emacs扩展性很好，但编写扩展需要特定的scripting language。所以atom想做成一款初学者和Hacker都会喜欢，随着他们能力增长而增长的编辑器。

### Atom基本功能

编辑器环境：

![1](http://biang.io/biangpic/blog/ed815a8faf3c070993ea5b275e4c8075.jpg)

可以看到，atom和sublime text长得非常相似，几乎是一个模子里出来的。

![2](http://biang.io/biangpic/blog/1854d10df7cc77b8b174e6bffaa700fa.jpg)

在atom里，command+shift+P和command+t就是你初期所需掌握的一切。通过第一个快捷键，你可以调出一个命令行窗口，运行各种和菜单对应的功能；通过第二个快捷键，你可以方便地调出某个文件。

和sublime text一样，atom也提供了package和plugin。atom提供了随处可查的帮助方便程序员对其扩展，比如说，要写theme，你可以先看看atom的style guide：

![3](http://biang.io/biangpic/blog/d56af8e8bc98a938490d8d967f192e3c.jpg)

只要你用过任何一款文本编辑环境，如sublime text, ultra edit等，那么上手atom很快，几乎不用任何学习。不过你也许会有疑惑：atom有什么好处？

### Atom插件系统

github在http://github.com/atom下开源了很多atom的package。我们随便看一个和排序相关的package —— 它能够排序选择的文本。使用起来是这个样子：

![4](http://biang.io/biangpic/blog/f6bbdf032eea0b2991b238587e36c600.jpg)

这个功能极其简单，对于一个程序员来说，只要告诉他用户选择的是什么，最迟不超过半小时就能编码完成排序。我们看看 atom sort 的主代码：

![5](http://biang.io/biangpic/blog/8755d974391819cccc084a3d5d5db0f9.jpg)

非常简单直观的coffeescript代码。相信做过前端开发的工程师，atom选择试用coffeescript是个福音（atom的配置文件也是cson格式的）。

不要小看插件语言选择的重要性。sublime text为何有那么多第三方的package？我觉得和它的API，以及使用Python来开发插件很有关系。Python写出来的代码可读性不错，sublime text的API也比较清晰，这样有了一些package样板后，很容易激发后来者写出更多优秀的插件。这是vim/emacs所无法比拟的 —— python程序员要比vim script或者emacslisp程序员多多了！

atom选择coffeescript有几个很重要的考量（我猜的）：

* coffeescript(javascript)的拥趸很多
* 语言强大，代码简单
* 是主流能够运行在浏览器中的语言（coffeescript需要先翻译成javascript）

拿到atom后，我一直在怀疑它是个运行在浏览器中的web app。看看这个界面，是不是很有chrome的赶脚？？

![6](http://biang.io/biangpic/blog/b37b7085ab498b0bd519281bcbe9d408.jpg)

这就是Atom最大的亮点！web native。在此之下，less style，coffeescript plugin，nodejs integration都水到渠成。看上去atom的源代码来自chrome —— 我觉得atom很可能是一款以某种方式运行本地web app的浏览器。chrome的源代码base在webkit上（貌似是bsd），所以atom可以任意修改。很可能chrome上面的沙箱环境（不允许web app访问本地资源，如文件系统）被移除，然后nodejs以某种方式被集成进来（这样javascript可以访问文件系统等本地资源）。

### 对Atom的思考

web正在迅速吞噬一切。PhoneGap等工具已经在手机客户端上使用WebUI部分取代native app（尽管长路漫漫）。我觉得这个过程是一个趋势，就像C逐渐将asm挤出主流应用一样。桌面的应用也在往这个趋势上走。

当然你可以argue说web app无法发挥native的所有性能和硬件能力。我不否认。但是，很多通用的软件不需要这些东西。如果能用javascript花一个小时写出来，再用phoneGap等工具一编译就搞定的活，使用native code写个todo list意义又何在呢？同样的道理适用于desktop app。

Web的魅力在于可扩展性。对于浏览器而言，html/css/javascript是套完整的API。浏览器不关心最终渲染出来什么东西，只要给它的输入符合这套API，它就能很好地解析。这带来了几乎无限的扩展性 —— 而这种扩展性，不正是我们做软件的梦寐以求的么？所以，在这套优秀框架的带动下，web将疆界几乎延伸到IT的所有领域中。

形成这个趋势的一个重要原因是：native app 做下去做的仅仅是一个封闭的application；而web app做下去往往做的是一个开放的system。

但web的问题在于你无法掌控entry point。如果你的app不被bookmark，那么用户下次记得你的代价就比较大。这是为什么会有phoneGap这样看上去很奇怪的hybrid出现。这也是atom出现的原因之一。

在mobile上，使用浏览器的代码做基石，而不是web view的代价可能比较大，比如说文件大小，成熟度等。但在desktop上，这个可行性大了不少，因为开源的chrome的生态圈很成熟。

Atom之后（如果这种它收效很好），未来桌面应用很可能会类似使用webkit（chrome）做壳，然后大部分功能都构建在web app（html/css/javascript）的结构之中。这样的结构灵活性，扩展性都很强，而且不用重启软件就可以升级功能。

### Atom的问题

就目前两小时的使用而言，我遇到了如下问题：

(1) 第一次打开atom的时候，一个help文档会被自动打开。正当我一行行看的时候，atom自己crash了。所以目前beta的atom似乎还是很不稳定。

(2) 输入速度有点慢，有迟滞感（和web有关？）。

(3) 中文支持总觉得有点问题，虽然没有遇到问题。

(4) 谁知道line wrap怎么设置？我设了但似乎不好使。一段文字挤在一行里感觉很不爽。这是目前我的block issue。

(5) 目前只有osx版本。

另外，尽管在软件层面尽量拥抱web世界，但如果最终atom不免费/开源，那么和sublime text竞争还是会比较吃力 —— 毕竟sublime text已经形成了极好的市场口碑和生态圈。大多数用户不关心你用什么技术，而关心我要的功能你有没有。

戳链接看atom官网，尝鲜申请体验atom吧：[atom.io](https://atom.io)。
