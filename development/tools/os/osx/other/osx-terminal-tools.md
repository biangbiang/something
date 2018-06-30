8个不可不知的Mac OS X专用命令行工具
===================================

OS X的终端下通用很多Unix的工具和脚本。如果从Linux迁移到OS X会发现很多熟悉的命令和脚本工具，其实并没有任何区别。

但是OS X也提供了很多其他系统所没有的特别的命令行工具。我们推荐8个这类的工具，希望有助于提高在Mac的命令行环境下的效率。

### 1. open

open命令用于打开文件、目录或执行程序。就等同于在命令行模式下，重复图形界面“双击”的动作。例如这个命令与在Finder中双击Safari是一样的：

    $ open /Applications/Safari.app/

如果open一个文件，则会使用关联的程序打开之。例如open screenshot.png会在Preview中查看图片。

可以使用-a选项要求自行选择打开的程序，或使用-e强制在TextEdit中编辑此文件。

open一个目录会在Finder窗口中打开此目录。一个很有用的技巧是open .打开当前目录。

Finder和终端的交互是双向的——把文件从Finder中拖入终端，就等同于把文件的完整路径粘贴到命令行中。

### 2. pbcopy 和 pbpaste

这两个工具可以打通命令行和剪贴板。当然用鼠标操作复制粘贴也可以——但这两个工具的真正威力，发挥在将其用作Unix工具的时候。意思就是说：可以将这两个工具用作管道、IO重定向以及和其他命令的整合。例如：

    $ ls ~ | pbcopy

可以将主目录的文件列表复制到剪贴板。

也可以把任意文件的内容读入剪贴板：

    $ pbcopy < blogpost.txt

做点更疯狂的尝试：获取最新Google纪念徽标（doodle）的URL并复制到剪贴板：

    $ curl http://www.google.com/doodles#oodles/archive | grep -A5 'latest-doodle on' | grep 'img src' | sed s/.*'<img src="\/\/'/''/ | sed s/'" alt=".*'/''/ | pbcopy

使用管道语法配合pbcopy工具可以简单的抓取命令的输出，而不必向上滚动翻阅终端窗口。可以用于和他人分享命令行的标准和错误输出。pbcopy和pbpaste也可以用于自动化或加速执行一些事情。例如把一些邮件的主题存为任务列表，就可以先从Mail.app中复制主题，再运行：

    $ pbpaste >> tasklist.txt

### 3. mdfind

许多Linux用户都发现Linux下查找文件的方法在OS X上不好用。当然经典的Unix find命令总是可以，但既然OS X有杀手级搜索工具Spotlight，为什么不在命令行上也使用一下呢？

这就是mdfind命令了。Spotlight能做的查找，mdfind也能做。包括搜索文件的内容和元数据（metadata）。

mdfind还提供更多的搜索选项。例如-onlyin选项可以约束搜索范围为一个目录：

    $ mdfind -onlyin ~/Documents essay

mdfind的索引数据库在后台自动更新，不过你也可以使用mdutil工具诊断数据库的问题，诊断mdfind的问题也等同于诊断Spotlight。如果Spotlight的工作不正确，mdutil -E命令可以强制重建索引数据库。也可以用mdutil -i彻底关闭文件索引。

### 4. screencapture

screencapture命令可以截图。和Grab.app与cmd + shift + 3或cmd + shift + 4热键相似，但更加的灵活。

抓取包含鼠标光标的全屏幕，并以image.png插入到新邮件的附件中：

    $ screencapture -C -M image.png 

用鼠标选择抓取窗口（及阴影）并复制到剪贴板：

$ screencapture -c -W

延时10秒后抓屏，并在Preview中打开之：

    $ screencapture -T 10 -P image.png

用鼠标截取一个矩形区域，抓取后存为pdf文件：

    $ screencapture -s -t pdf image.pdf

更多用法请参阅screencapture --help。

### 5. launchctl

launchctl管理OS X的启动脚本，控制启动计算机时需要开启的服务。也可以设置定时执行特定任务的脚本，就像Linux cron一样。

例如，开机时自动启动Apache服务器：

    $ sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist

运行launchctl list显示当前的启动脚本。sudo launchctl unload [path/to/script]停止正在运行的启动脚本，再加上-w选项即可去除开机启动。用这个方法可以一次去除Adobe或Microsoft Office所附带的所有“自动更新”后台程序。

Launchd脚本存储在以下位置：

    ~/Library/LaunchAgents    
    /Library/LaunchAgents          
    /Library/LaunchDaemons
    /System/Library/LaunchAgents
    /System/Library/LaunchDaemons

启动脚本的格式可以参考这篇blog，或苹果开发者中心的文章。你也可以使用Lingon应用来完全取代命令行。

### 6. say

say是一个文本转语音（TTS）的有趣的工具，引擎和OS X使用的一样也是VoiceOver。如果不加其他选项，则会简单的语音朗读你给定的字符串：

    $ say "Never trust a computer you can't lift."

用-f选项朗读特定文本文件，-o选项将朗读结果存为音频文件而不是播放：

    $ say -f mynovel.txt -o myaudiobook.aiff

say命令可以用于在脚本中播放警告或提示。例如你可以设置Automator或Hazel脚本处理文件，并在任务完成时用say命令语音提示。

最好玩（不过也负罪感十足）的用法是：通过SSH连接到朋友或同事的计算机，然后用say命令给他们一个大大大惊喜……

可以在系统设置（System Preferences）的字典和语音（Dictation & Speech）选项中调整系统的语音选项甚至是语音的语言。

### 7. diskutil

diskutil是OS X磁盘工具应用的命令行版。既可以完成图形界面应用的所有任务，也可以做一些全盘填0、全盘填随机数等额外的任务。先使用diskutil list查看所有磁盘的列表和所在路径，然后对特定的磁盘执行命令。

警告：不正确使用diskutil可能意外的破坏磁盘数据。请小心。

### 8. brew

Homebrew程序提供的brew，严格来讲不是一个OS X的原生命令，但任何一个OS X的专业用户都不会错过它。“OS X缺少的包管理器”这个评价是恰如其分的。如果你曾经在Linux上使用过apt-get（或其他包管理器——译者注），你就会发现Homebrew基本上是一样的。

使用brew可以简单的获取数千种开源工具和函数库。例如brew install imagemagick就可以安装ImageMagick（几乎可以处理任何图像问题，转换任何格式的图像工具），brew install node可以安装Node.js（当前大热的服务器端JavaScript编程工具）。

也可以通过Homebrew做有趣的事情：brew install archey会安装Archey（在启动命令行时显示苹果LOGO和计算机硬件参数的小工具）。

![](http://biang.io/biangpic/blog/56594c5a2a5bbf749b6d97d06c6fcfc3.png)

Homebrew能安装的工具数量庞大，并且一直保持更新。Homebrew最棒的一点是：所有的文件都被约束在/usr/local/一个位置之下。也就是说可以通过Homebrew安装新版软件的同时，保持系统内置的依赖库或其他软件不变。同时如果想彻底删除Homebrew，也变得非常简单。

（注：删除Homebrew最好还是不要直接删除/usr/local/。应当用这个[卸载脚本](https://gist.github.com/mxcl/1173223)。）

最后，这里还有一篇OS X所有命令的[完整列表](http://ss64.com/osx/)。
