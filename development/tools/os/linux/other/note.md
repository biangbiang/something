linux学习笔记
===========

## 管道, 重定向和 backtick

这些不是系统命令，但是他们真的很重要

### 管道（｜）

管道是Linux中很重要的一种通信方式,是把一个程序的输出直接连接到另一个程序的输入,常说的管道多是指无名管道,无名管道只能用于具有亲缘关系的进程之间，这是它与有名管道的最大区别。

* 无名管道
* 有名管道

### 重定向

将命令的结果输出到文件，而不是标准输出(屏幕)

* 〉写入文件并覆盖旧文件
* 〉〉输出追加到文件的尾部，保留旧文件。

### backtick

反短斜线,使用反短斜线可以将一个命令的输出作为另外一个命令的一个命令行参数。

---

### pwd

* 全称：Print Working Directory

* 用途：显示工作目录的路径名称

### date

查看当前机器的时间

* date -R: 查看当前时区

### uniq

uniq 命令删除文件中的重复行。 uniq 命令读取由 InFile 参数指定的标准输入或文件。该命令首先比较相邻的行，然后除去第二行和该行的后续副本。

###  报错can't set the locale; make sure $LC\_* and $LANG are correct

1. 在 /etc/environment 里加入一行 LC\_ALL="en\_US.UTF-8"
2. sudo locale-gen en\_US.UTF-8
3. sudo reboot