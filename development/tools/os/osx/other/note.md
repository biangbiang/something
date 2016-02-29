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

---

### 苹果机支持的分区表类型

Mac系统支持如下三种硬盘分区:

* GUID

是基于Intel处理器苹果电脑使用的新的分区表, 也叫GUID Partition Table, GPT，是EFI标准的一个部分，详见WikiPedia的解释：GUID Parttion Table 。

在Intel处理器Mac电脑上可以使用GUID和APM分区表硬盘来启动机器，GUID是苹果建议使用的分区表格式。

对于使用时间机器备份的硬盘，只能使用GUID格式的分区表。

在PowerPC处理器机器上不能做启动盘，要用APM分区表格式，见下.

* APM

是Apple的标准，是Apple Partition Map的缩写，对于基于PowerPC的电脑，硬盘只能使用这种分区表格式才能作为系统启动盘。如果在分区中安装Universal Binary码的OS X系统, 基于Intel的电脑也可以从这样的分区表的启动分区硬盘启动.

如果考虑到系统兼容性(PowerPC和Intel-based)，那么硬盘应该使用这种分区表格式.

* MBR

Master Boot Record的缩写, 实际上是指硬盘上的第一个磁盘块。MBR是比较古老的标准，有诸多限制，比如最多支持4个主分区等，由于PC兼容机的广泛使用，以及微软一直没有放弃使 用，所以这种分区表格式还在PC世界存在。如果在苹果机上给硬盘使用这种分区表格式，一般都是应用在外置硬盘或者Ｕ盘上, 这样可以照顾到PC机使转移数据更方便. Mac的OS X系统不能从这种的分区表格式的硬盘上启动系统.
