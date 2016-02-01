mac学习笔记
===========

### 输入法

按住shift＋delete 就可以将第一个字去掉首位

---

### safari浏览器扩展

工行的登录控件好像用safari的扩展插件管理器看不到，无法删除，插件路径在`/Library/Internet Plug-Ins`下，可以自己手动删

* `NPCleanHistory.plugin`
* `NPClientBinding.plugin`
* `NPSafeInput.plugin`
* `NPSafeSubmit.plugin`

将这四个文件夹删除应该就可以了

---

### 在 OS X 下压缩中文名的文件，默认的 ZIP 格式，传给 Windows 用户后所有中文都乱码了，如何解决乱码问题？有什么压缩软件可以让文件在 Windows 下解压不乱码吗？

乱码原因：Mac OS X 系统自带的压缩程序对 zip 文件名用 UTF-8 编码，但 zip 文件头中没有声明 PKZIP 高版本增加的 Unicode 位。Windows 会认为文件名是 ANSI 编码，结果显示乱码。

解决方法：用第三方软件。我推荐 Keka，支持格式多，加密、分卷、压缩比调节等功能都有，还支持中文菜单和界面。最重要的：免费。

---

### mac自带的邮箱客户端能够修改账户类型么

暂时不能，只能在新建账户的时候设置账户类型是POP3还是IMAP

---

### mac os x 终端terminal打开速度很慢

##### 原因：

大量日志累计造成的

##### 解决：

    sudo rm -rf /private/var/log/asl/*.asl

---

### mac中的top命令

我相信很多开发人员选择mac除了它有很酷的界面外，它还是unix系统，在系统内置了很多unix的程序。

top就是很常用的一个程序，使用top的原因有以下：

1. 虽然mac中有活动监视器，但top命令来的快

2. gui程序不一定任何时刻都能正常运行，我亲身经历过强制退出没有效果，但可以在命令行 kill -9 <pid>（当然qq视频那个现象就无语了，直接将整个屏幕占满，任何操作都无效，只能眼睁睁的看着鼠标可以移动，但干不了任何事情）

之前一直依着linux的习惯在top运行时按M（按使用内存排序）、P（按CPU占用率排序）以及h（帮助），但在mac下却没有效果。我想可能mac下的top不支持这些吧，每次用完就不管了，也懒的google和man，今天忽然想到了在top运行时按?键，结果真出现了帮助菜单。

    o<key>         Set primary sort key to <key>: [+-]
    {command|cpu|pid|prt|reg|rprvt|rshrd|rsize|th|time|uid|username|vprvt|vsize}.

按CPU排序是ocpu，按内存排序是ovsize。
