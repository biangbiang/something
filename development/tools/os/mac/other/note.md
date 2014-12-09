mac学习笔记
=========

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
