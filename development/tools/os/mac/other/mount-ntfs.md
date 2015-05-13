Mac 挂载NTFS移动硬盘进行读写操作 （Read-only file system）
=========================================================

1. diskutil info /Volumes/YOUR\_NTFS\_DISK\_NAME 

  找到 Device Node
  
  Device Node:              /dev/disk1s1

2. hdiutil eject /Volumes/YOUR\_NTFS\_DISK\_NAME

  "disk1" unmounted.
  
  "disk1" ejected.
  
  弹出你的硬盘

3. 创建一个目录，稍后将mount到这个目录 

	    sudo mkdir /Volumes/MYHD

4. 将NTFS硬盘 挂载 mount 到mac

		sudo mount_ntfs -o rw,nobrowse /dev/disk1s1 /Volumes/MYHD/

5. 操作结束，卸载

		sudo umount
