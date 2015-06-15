在 Mac OS X 下如何完全关闭 Finder ?
===================================

所有程序都可以用Command+q的快捷键关闭掉,唯独Finder只能用鼠标点击左上角的关闭或者Command-W的快捷键关闭窗口，那如何完全退出？

Finder是os x的一个程序，跟其他程序一样可以关闭，但是系统默认关闭了这个选项，你可以自己打开，在终端输入：

    defaults write com.apple.finder QuitMenuItem -bool YES
    或
    defaults write com.apple.Finder QuitMenuItem 1

重启Finder，在 『终端』输入以下命令：

    killall Finder

然后Command+q的快捷键就可以关闭Finder了，但是桌面图标也没了。
