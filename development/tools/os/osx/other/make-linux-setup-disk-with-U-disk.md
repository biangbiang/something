mac上制作linux系统U盘安装盘
===========================

第一步，先插入U盘，打开终端使用下面的命令查看U盘是否已经mount到系统，这时在Finder下也能看到U盘。

    diskutil list

系统输出类似如下内容：

    star@star:Volumes$ diskutil list
    /dev/disk0
       #:                       TYPE NAME                    SIZE       IDENTIFIER
       0:      GUID_partition_scheme                        *160.0 GB   disk0
       1:                        EFI                         209.7 MB   disk0s1
       2:                  Apple_HFS Macintosh HD            159.2 GB   disk0s2
       3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
    /dev/disk1
       #:                       TYPE NAME                    SIZE       IDENTIFIER
       0:     Apple_partition_scheme                        *17.3 MB    disk1
       1:        Apple_partition_map                         32.3 KB    disk1s1
       2:                  Apple_HFS Flash Player            17.3 MB    disk1s2
    /dev/disk2
       #:                       TYPE NAME                    SIZE       IDENTIFIER
       0:     FDisk_partition_scheme                        *15.7 GB    disk2
       1:             Windows_FAT_32 KINGSTON                15.7 GB    disk2s1

上面的/dev/disk2即是U盘挂载点。

接下来先unmount这个U盘，使用如下命令：

    diskutil unmountDisk /dev/disk2

这样就有了一个已经插入但是unmount的U盘了，这时候你在Finder下看不到这个U盘了，但是用diskutil list命令还可以看到。

接下来将你要制作的Linux安装的iso文件写到这个U盘里，这个任务可以使用如下命令完成：

    dd if=~/Downloads/debian-6.0.6-i386-CD-1.iso of=/dev/disk2 bs=1m

这个过程大约需要5-10分钟，完成之后会输出类似下面的日志。

    648+0 records in
    648+0 records out
    679477248 bytes transferred in 447.883182 secs (1517086 bytes/sec)

这样一个通过USB启动的Linux安装U盘就做好了。接下来就是安装系统等事情了，这些事情就不细说了。
