linux学习笔记
===========

## 管道, 重定向和 backtick

这些不是系统命令，但是他们真的很重要

---

### 管道（｜）

管道是Linux中很重要的一种通信方式,是把一个程序的输出直接连接到另一个程序的输入,常说的管道多是指无名管道,无名管道只能用于具有亲缘关系的进程之间，这是它与有名管道的最大区别。

* 无名管道
* 有名管道

---

### 重定向

将命令的结果输出到文件，而不是标准输出(屏幕)

* 〉写入文件并覆盖旧文件
* 〉〉输出追加到文件的尾部，保留旧文件。

---

### backtick

反短斜线,使用反短斜线可以将一个命令的输出作为另外一个命令的一个命令行参数。

---

### pwd

* 全称：Print Working Directory

* 用途：显示工作目录的路径名称

---

### date

查看当前机器的时间

* date -R: 查看当前时区

---

### uniq

uniq 命令删除文件中的重复行。 uniq 命令读取由 InFile 参数指定的标准输入或文件。该命令首先比较相邻的行，然后除去第二行和该行的后续副本。

---

###  报错can't set the locale; make sure $LC\_* and $LANG are correct

1. 在 /etc/environment 里加入一行 LC\_ALL="en\_US.UTF-8"
2. sudo locale-gen en\_US.UTF-8
3. sudo reboot

---

### linux查看本机IP、gateway、dns

#### IP: 

    ifconfig

#### gateway:

    [root@localhost ~]# netstat -rn
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags   MSS Window irtt Iface
    172.16.44.0     0.0.0.0          255.255.255.0    U         0      0          0 vmnet8
    172.16.10.0     0.0.0.0          255.255.255.0        U         0      0          0 vmnet1
    172.16.0.0       0.0.0.0          255.255.252.0        U         0      0          0 eth0
    169.254.0.0     0.0.0.0          255.255.0.0           U         0      0          0 eth0
    0.0.0.0         172.16.0.254    0.0.0.0           UG        0      0          0 eth0

(以0.0.0.0开始的行的gateway是默认网关)

#### DNS:

    [root@localhost ~]# cat /etc/resolv.conf
    search               localdomain
    nameserver 172.16.0.250

### 修改密码

你是普通用户的话，修改自己的密码，用：passwd，就可以了，会让你先输入自己的旧密码，再输入两遍新密码。

你是root的话，用：passwd username，就可以修改username的密码了，直接输入两遍新密码就可以了，不用输入旧密码。
