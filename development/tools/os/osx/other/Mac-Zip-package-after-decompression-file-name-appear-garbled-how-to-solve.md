# Mac 下 Zip 压缩包解压后文件名出现乱码，怎么解决？

## Mac 下很多时候 ZIP 格式的压缩包解压之后，文件名出现乱码，如何才能避免？

Mac OS 下，Zip 格式的压缩包解压之后出现乱码，是因为别人压缩制作这个 Zip 压缩包的时候，用的是非UTF-8编码（如Windows中文版的默认字符编码为 GB2312），而 Mac 自带的 归档实用工具 解压 Zip 格式文件的时候使用的字符编码是 Mac OS 默认的字符编码（UTF-8），所以导致解压后的文件名出现乱码。

## 解决方法

使用解压软件 `The Unarchiver` 来解压 Zip 格式的文件。

注：TheUnarchiver 支持自动检测文件名编码，当 The Unarchiver 不能确定使用哪一种字符编码的时候，会弹出如下所示的选择窗口来提醒用户选择一个合理的字符编码用于解压，所以几乎不会出现解压 Zip 乱码的情况。

![](http://biang.io/biangpic/blog/f914922d0d527a262e27d4c48a6ef51b.png)

![](http://biang.io/biangpic/blog/16adae5361d1989b18fa3db003424fa7.png)

![](http://biang.io/biangpic/blog/718be1e485b7b0896ad7dc402769ed0d.png)

### 第 1 步 下载、安装 The Unarchiver

下载地址1（官方最新版，需要用 App Store 下载）：http://itunes.apple.com/app/the-unarchiver/id425424353?mt=12&ls=1

下载地址2（非最新版，版本为 v3.10.1，百度云 网盘）：http://pan.baidu.com/s/1o7zTteq

如果是通过 App Store 下载安装的，The Unarchiver 可以在 Finder—应用程序 目录下找到；如果是从 百度云 网盘下载的，将会得到 dmg 文件，打开之后复制到 Finder—应用程序 目录下

### 第 2 步 设置 The Unarchiver 为 Zip 格式文件的默认解压工具

打开 The Unarchiver，点击选项卡 压缩格式，勾选 Zip Archive，点击左上角的关闭按钮退出 The Unarchiver，如下图：

![](http://biang.io/biangpic/blog/3893c4377e21798502d711a822e25ac0.png)

### 完成

之后解压 Zip 格式的压缩包的时候，都会自动使用 The Unarchiver 来解压了。

### 提示

解压之后得到的文件或文件夹，会直接保存在 Zip 压缩包所在的文件夹。

如果不想设置 The Unarchiver 为 Zip 格式文件的默认解压工具，可以右击 Zip 压缩包，选择 打开方式，选择 The Unarchiver 来解压 Zip，如下图：

![](http://biang.io/biangpic/blog/68a7551f4c207ddc81732f50f60cf381.png)
