linux nano的使用
===============

Nano是一种单模式编辑器，你可以直接输入文字。如果你要编辑一个像/etc/fstab 一样的配置文件，请使用-w 参数，例如：

警告: 这非常重要。如果在编辑配置文件时忘了加-w 参数，可能会导致你的系统无法起动或产生别的异常。

### 保存和退出

如果你要保存所做的修改，按下Ctrl+O 。想要退出，按下Ctrl+X 。如果你退出前没有保存所做的修改，它会提示你是否要保存。如果不要，请按N ，反之，则按Y 。然后它会让你确认要保存的文件名，确认或修改后按Enter 即可。

如果你没有修改好而不小心按了保存键，您可以在请求确认文件名时按Ctrl+C 来取消。

### 剪切和粘贴

要剪切一整行，请用Ctrl+K （按住Ctrl 不放，再按下K 键）。光标所在的行便消失了。要粘贴它，只需把光标移动到您所要粘贴的位置，然后按Ctrl+U 即可。要移动多行，只需多按几次Ctrl+K 把需要移动内容都剪切下来，然后按一次Ctrl+U 就可以把刚剪切的内容全部粘贴上来。

如果你想使用更精确的剪切控制，则需要给文本做标记。移动光标到需要剪切文本的开头，按下Ctrl+6 （或者Alt+A ）。然后移动光标到待剪切文本的末尾：被做了标记的文本便会反白。要撤消文本标记，只需再按一次Ctrl+6 。用Ctrl+K 来剪切被标记的文本，用Ctrl+U 来粘贴。

### 搜索特定文字

当你想搜索某特定文字时，只要想成"WhereIs" 而不是"Search" ，事情就简单了。只要按下Ctrl+W ，键入你要搜索的字符串，再按Enter 就可以了。想再次搜索相同的字符串，可以直接按Alt+W 。

注意: 在nano帮助文档里，Ctrl-键被表示为一个脱字符（^ ），因此Ctrl+W 被写成了^W ，等等。Alt-键被表示为一个M （从"Meta"而来），因此Alt+W 被写成了M-W 。

---

一直以来vi都被人们说是最强大的编辑器，但gentoo和debian选择nano做了默认的编辑器

freebsd选择ee做了默认的编辑器，我相信在专业人士眼睛里面freebsd和redaht比较起来，

RedHat 基本没什么可以炫耀的，为什么他们不选择vi呢，因为vi操作比较复杂

而所谓的简单编辑器nano就简单，非常容易上手，说是简单编辑器

其实一点都不简单，只不过是nano谦虚一下罢了

这里声明一下^表示键盘上的ctrl键，上个只要是做过编程的朋友应该都清楚，^G表示同时按下ctrl和g

(F1)表示按(F1)也是一样的 ，M-表示使用alt+后面的键

^G ==(F1) Invoke the help menu

调用帮助菜单

^X ==(F2) Close currently loaded file/Exit from nano

退出

^O ==(F3) Write the current file to disk == ^O WriteOut

保存

然后回车就保存了

^J ==(F4) Justify the current paragraph

调整当前段落（配置文件的不要用这东西，格式一下就出问题了哦）

^R ==(F5) Insert another file into the current one

插入其他的文件到当前的文件，而且查找文件的时候支持tab

^W ==(F6) Search for text within the editor

查找

^Y ==(F7) Move to the previous screen

上一屏幕

^V ==(F8) Move to the next screen

下一屏幕

^K ==(F9) Cut the current line and store it in the cutbuffer

裁减当前一排并保存在缓冲区

^U ==(F10) Uncut from the cutbuffer into the current line

将缓冲区的东西粘贴到此

^C ==(F11) Show the position of the cursor

显示光标位置

^T ==(F12) Invoke the spell checker, if available

调用拼写检查程序

^P Move up one line

向上移动一行

^N Move down one line

向下移动一

^F Move forward one character

向前移动光标一格

^B Move back one character

向后移动光标一格

^A Move to the beginning of the current line

移动到当前行的开头

^E Move to the end of the current line

移动到当前行的末尾

^L Refresh (redraw) the current screen

刷新当前屏幕

^^ (M-A) Mark text at the current cursor location

标记文本

^D Delete the character under the cursor

删除光标后一个字母

^H Delete the character to the left of the cursor

向左边删一个字母

^I Insert a tab character

插入一个tab值

^\ (F14) (M-R) Replace text within the editor

查找并且替换

^M Insert a carriage return at the cursor position

插入一个回车

^_ (F13) (M-G) Go to a specific line number

跳转到某行

^Space Move forward one word

前进一个单词

M-Space Move backward one word

后退一个单词

M-] Find other bracket

搜索下一个括号

M-< Open previously loaded file

打开先前加载的文件

M-> Open next loaded file

打开下一个加载的文件

M-C Constant cursor position enable/disable

M-I Auto indent enable/disable

是否首行缩进

M-Z Suspend enable/disable

是否悬挂

M-X Help mode enable/disable

帮助模式

M-M Mouse support enable/disable

鼠标支持

M-Y Color syntax highlighting enable/disable

语法加亮

这个就是退出了哦

好了nano 的编辑器就说这样多了
