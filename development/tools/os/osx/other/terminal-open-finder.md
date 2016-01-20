在MAC终端下打开Finder
=====================

对于用习惯了terminal的mac user来说,经常会遇到进入到某个目录,但是需要在finder中打开这个目录的需要.

之前一直很苦恼,明明已经切换到相应的目录了,还得在finder中一个个的路径找,很是不方便.

经过一番搜索,终于找到了相应的方法:

    open .

通过查看man手册,可以看到open是一个MAC OSX下特有的命令,其作用就是用来打开文件.

    The open command opens a file (or a directory or URL), just as if you had double-clicked the >file’s icon. If no application name is specified, the default application as determined via LaunchServices is used to open the specified files.
    If the file is in the form of a URL, the file will be opened as a URL.
    You can specify one or more file names (or pathnames), which are interpreted relative to the shell or Terminal window’s current working directory. For example, the following command would open all Word files in the current working directory:
    open *.doc
    Opened applications inherit environment variables just as if you had launched the application directly through its full path. This behavior was also present in Tiger.

看了一下,其也可以打开相应的URL.

    summertekiMacBook-Pro:server summer$ open http://www.baidu.com

-

    <span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">其调用了系统默认的程序打开路径.在这里,</span><strong style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">http://</strong><span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">是必要的.其指明了这是一个URL,否则会被解析为一个文件.</span>

另外,其也支持很多操作.比如:

指定打开文件的程序: -a

    open -a Safari http://www.baidu.com

-

    <span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">调用TextEdit编辑文件: -e</span>

-

    open -e main.c

-

    <span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">打开finder,并且以某个文件为焦点</span>

-

    open -R main.c

-

    <code style="font-size: 13px; line-height: 19px;">open</code><span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">这个命令解决了我很久的苦恼,打开文件夹终于不再那么困难了.</span>
