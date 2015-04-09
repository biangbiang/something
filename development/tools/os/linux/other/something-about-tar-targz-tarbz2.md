关于tar、tar.gz、tar.bz2的区别与用法
====================================

### tar命令

tar 可以为文件和目录创建档案。利用tar，用户可以为某一特定文件创建档案（备份文件），也可以在档案中改变文件，或者向档案中加入新的文件。tar最初被 用来在磁带上创建档案，现在，用户可以在任何设备上创建档案，如软盘。利用tar命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将 几个文件组合成为一个文件以便于网络传输是非常有用的。Linux上的tar是GNU版本的。

语法：tar [主选项+辅选项] 文件或者目录

使用该命令时，主选项是必须要有的，它告诉tar要做什么事情，辅选项是辅助使用的，可以选用。

主选项：

* c 创建新的档案文件。如果用户想备份一个目录或是一些文件，就要选择这个选项。
* r 把要存档的文件追加到档案文件的未尾。例如用户已经作好备份文件，又发现还有一个目录或是一些文件忘记备份了，这时可以使用该选项，将忘记的目录或文件追加到备份文件中。
* t 列出档案文件的内容，查看已经备份了哪些文件。
* u 更新文件。就是说，用新增的文件取代原备份文件，如果在备份文件中找不到要更新的文件，则把它追加到备份文件的最后。
* x 从档案文件中释放文件。

辅助选项：

* b 该选项是为磁带机设定的。其后跟一数字，用来说明区块的大小，系统预设值为20（20*512 bytes）。
* f 使用档案文件或设备，这个选项通常是必选的。
* k 保存已经存在的文件。例如我们把某个文件还原，在还原的过程中，遇到相同的文件，不会进行覆盖。
* m 在还原文件时，把所有文件的修改时间设定为现在。
* M 创建多卷的档案文件，以便在几个磁盘中存放。
* v 详细报告tar处理的文件信息。如无此选项，tar不报告文件信息。
* w 每一步都要求确认。
* z 用gzip来压缩/解压缩文件，加上该选项后可以将档案文件进行压缩，但还原时也一定要使用该选项进行解压缩。 

### Linux下的压缩文件剖析 

对于刚刚接触Linux的人来说，一定会给Linux下一大堆各式各样的文件名给搞晕。别个不说，单单就压缩文件为例，我们知道在Windows下最常见的 压缩文件就只有两种，一是,zip，另一个是.rap。可是Linux就不同了，它有.gz、.tar.gz、tgz、bz2、.Z、.tar等众多的压 缩文件名，此外windows下的.zip和.rar也可以在Linux下使用，不过在Linux使用.zip和.rar的人就太少了。本文就来对这些常 见的压缩文件进行一番小结，希望你下次遇到这些文件时不至于被搞晕:) 

在具体总结各类压缩文件之前呢，首先要 弄清两个概念：打包和压缩。打包是指将一大堆文件或目录什么的变成一个总的文件，压缩则是将一个大的文件通过一些压缩算法变成一个小文件。为什么要区分这 两个概念呢？其实这源于Linux中的很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你就得先借助另它的工具将这一大堆文件先打 成一个包，然后再就原来的压缩程序进行压缩。 

Linux下最常用的打包程序就是tar了，使用tar程序打出来的包我们常称为tar包，tar包文件的命令通常都是以.tar结尾的。生成tar包后，就可以用其它的程序来进行压缩了，所以首先就来讲讲tar命令的基本用法：

tar命令的选项有很多(用man tar可以查看到)，但常用的就那么几个选项，下面来举例说明一下：

    # tar -cf all.tar *.jpg

这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

    # tar -rf all.tar *.gif

这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

    # tar -uf all.tar logo.gif

这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。 

    # tar -tf all.tar 

这条命令是列出all.tar包中所有文件，-t是列出文件的意思

    # tar -xf all.tar

这条命令是解出all.tar包中所有文件，-t是解开的意思

以上就是tar的最基本的用法。为了方便用户在打包解包的同时可以压缩或解压文件，tar提供了一种特殊的功能。这就是tar可以在打包或解包的同时调用其它的压缩程序，比如调用gzip、bzip2等。

1) tar调用gzip

gzip是GNU组织开发的一个压缩程序，.gz结尾的文件就是gzip压缩的结果。与gzip相对的解压程序是gunzip。tar中使用-z这个参数来调用gzip。下面来举例说明一下：

    # tar -czf all.tar.gz *.jpg

这条命令是将所有.jpg的文件打成一个tar包，并且将其用gzip压缩，生成一个gzip压缩过的包，包名为all.tar.gz

    # tar -xzf all.tar.gz

这条命令是将上面产生的包解开。

2) tar调用bzip2

bzip2是一个压缩能力更强的压缩程序，.bz2结尾的文件就是bzip2压缩的结果。与bzip2相对的解压程序是bunzip2。tar中使用-j这个参数来调用gzip。下面来举例说明一下：

    # tar -cjf all.tar.bz2 *.jpg

这条命令是将所有.jpg的文件打成一个tar包，并且将其用bzip2压缩，生成一个bzip2压缩过的包，包名为all.tar.bz2

    # tar -xjf all.tar.bz2

这条命令是将上面产生的包解开。

3)tar调用compress 

compress也是一个压缩程序，但是好象使用compress的人不如gzip和bzip2的人多。.Z结尾的文件就是bzip2压缩的结果。与compress相对的解压程序是uncompress。tar中使用-Z这个参数来调用gzip。下面来举例说明一下： 

    # tar -cZf all.tar.Z *.jpg

这条命令是将所有.jpg的文件打成一个tar包，并且将其用compress压缩，生成一个uncompress压缩过的包，包名为all.tar.Z

    # tar -xZf all.tar.Z

这条命令是将上面产生的包解开

有了上面的知识，你应该可以解开多种压缩文件了，下面对于tar系列的压缩文件作一个小结：

1)对于.tar结尾的文件

    tar -xf all.tar

2)对于.gz结尾的文件

    gzip -d all.gz
    gunzip all.gz

3)对于.tgz或.tar.gz结尾的文件

    tar -xzf all.tar.gz
    tar -xzf all.tgz

4)对于.bz2结尾的文件

    bzip2 -d all.bz2
    bunzip2 all.bz2

5)对于tar.bz2结尾的文件

    tar -xjf all.tar.bz2

6)对于.Z结尾的文件

    uncompress all.Z

7)对于.tar.Z结尾的文件

    tar -xZf all.tar.z

另外对于Window下的常见压缩文件.zip和.rar，Linux也有相应的方法来解压它们：

1)对于.zip

linux下提供了zip和unzip程序，zip是压缩程序，unzip是解压程序。它们的参数选项很多，这里只做简单介绍，依旧举例说明一下其用法：

    # zip all.zip *.jpg

这条命令是将所有.jpg的文件压缩成一个zip包

    # unzip all.zip 

这条命令是将all.zip中的所有文件解压出来 

2)对于.rar 

要在linux下处理.rar文件，需要安装RAR for Linux，可以从网上下载，但要记住，RAR for Linux不是免费的；然后安装：

    # tar -xzpvf rarlinux-3.2.0.tar.gz
    # cd rar
    # make

这样就安装好了，安装后就有了rar和unrar这两个程序，rar是压缩程序，unrar是解压程序。它们的参数选项很多，这里只做简单介绍，依旧举例说明一下其用法：

    # rar a all *.jpg

这条命令是将所有.jpg的文件压缩成一个rar包，名为all.rar，该程序会将.rar 扩展名将自动附加到包名后。

    # unrar e all.rar

这条命令是将all.rar中的所有文件解压出来

到 此为至，我们已经介绍过linux下的tar、gzip、gunzip、bzip2、bunzip2、compress、uncompress、zip、 unzip、rar、unrar等程式，你应该已经能够使用它们 对.tar、.gz、.tar.gz、.tgz、.bz2、.tar.bz2、.Z、.tar.Z、.zip、.rar这10种压缩文件进行解压了，以后 应该不需要为下载了一个软件而不知道如何在Linux下解开而烦恼了。而且以上方法对于Unix也基本有效。

本文介绍了linux下的压缩程式 tar、gzip、gunzip、bzip2、bunzip2、compress、uncompress、zip、unzip、rar、unrar等程 式，以及如何使用它们对.tar、.gz、.tar.gz、.tgz、.bz2、.tar.bz2、.Z、.tar.Z、.zip、.rar这10种压缩 文件进行操作。

---

初次用tar，会情不自禁的来上一条命令：

    tar -cvf yourdirectory

然后当然是报错了：tar：XXX ：Is a directory（XXX是一个目录），这是因为你没能理解tar是来干嘛的，请往下看，你就会茅塞顿开了。。。

You seem to misunderstand the difference between tar and bzip2.

tar is a file archive utility that takes a number of files, with pathnames, and puts them into a single binary file. It doesn't do anything to compress the data. 

bzip2 (like gzip and compress) is a file (or stream) compression utility, that takes a single file and makes it smaller. 

If you want to create an archive of a directory, you first need to create a tar archive, then bzip2 compress the archive; this is why “tarball”s often have the extension .tar.bz2 Code:tar -cf file.tar dir && bzip2 file.tar You can also do this in one step: Code:tar -cjf file.tar.bz2 dir This compresses better than all-in-one formats like ZIP, which compress each file seperately then append then put the compressed files into an archive.

If you want to compress each file within a directory, you can use Code:bzip2 dir/* To do this recursively you'll need to use find: Code:find dir -exec bzip2 '{}' ';'
