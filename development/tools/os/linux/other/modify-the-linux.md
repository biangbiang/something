# 修改Linux的时间 
 
修改Linux时间一般涉及到3个命令： date， clock， hwclock 

date： 修改系统当前的时间：  

    [root]#date –s '2005/12/5 10:01:00'  

系统当前的时间改成2005年12月5日，10点01分  

这个修改在系统重启后就失效了，因此为了将这个时间永久生效，需要将修改的时间写入CMOS。  

看CMOS的时间：  

    #clock –r 

将当前系统时间写入CMOS中去  

    #clock –w 
 
Linux机器上的时间比较复杂，有各式各样的时钟和选项等等。 
 
机器里有两个时钟。硬件时钟从根本上讲是CMOS时钟；而系统时钟是由内核维护的，它是从1969年末（即传说中的标志Unix时代开端的那个拂晓）开始算起的累积秒数。 
 
在DOS或Mac系统中，起作用的是硬件时钟。遗憾的是，你可能已经发现了，绝大多数电脑时钟都是很不准确的。它们从根本上讲是由小型电池供电的警报器时钟，这种锂电池一般可持续供电三年左右，那时候你系统各大块差不多都已经过时了。 
 
而在Linux系统中，起作用的是系统时钟。在启动时，它靠读取硬件时钟获得计时起点，而不是靠记忆计时。 
 
你可以通过BIOS修改系统硬件时钟，或者如果你不想重起机器，那就用hwclock命令。 
 
比较酷的一点是，当你使用hwclock命令调整硬件时间很多次以后，hwclock就会获取你的时钟推移速率，然后就会把这个信息存在/etc/adjtime里边。以后，你就可以用它来随时更新你的硬件时间，用一条下面的命令： 
 
    hwclock --adjust 
 
硬件时钟通常被设置成全球标准时间（UTC），而将时区信息保存在/usr/share/lib/timezone （或者在某些系统中可能是/usr/local/timezone）目录下某个适当的文件中，然后用一个符号链接文件/etc/localtime指向它。 
 
查看硬件时钟用命令： 
 
    hwclock --show 
 
重置硬件时钟用： 
 
    hwclock --set --date="1/23/01 22:16:59" 
 
如果需要修改你的时区信息，可以使用tzset命令，如果你系统中没有这条命令，那可以用类似下面的操作： 
 
    ln -s /etc/localtime /usr/share/zoneinfo/US/Pacific 
 
要掌握linux的时间操作还有很多东西需要了解，包括用来创建实时时钟文件（/dev/rtc）的内核选项、在内核或TZ时区表中设置时区信息的方法、网络时间服务器功能和夏令时等等。
