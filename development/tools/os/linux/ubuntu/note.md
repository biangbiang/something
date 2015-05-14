ubuntu学习笔记
==============

### apt-cache查找可用软件包

    apt-cache search {name}
    apt-cache depends ｛name｝//查找{name}依赖哪些包
    apt-get rdepends {name}  //查找哪些包依赖{name}

只有depends才能确定真正的依赖性，反向的依赖性不一定可靠。Depends开头的是必须安装的，Recommends不是必需的。如fping并不是mplayer是必需的。

另外apt-get install package时以Depends或是Recommends开头的行中的包都将被安装，而以Suggests开头的行中的包不会被安装。

---

### Ubuntu 修改源

打开Ubuntu的终端,输入

    sudo gedit /etc/apt/sources.list

删掉里边所有旧的内容，把新的源列表内容贴进去

再执行:

    sudo apt-get update

就可以生效。

ubuntu镜像源列表参考

    deb http://mirrors.163.com/ubuntu/ intrepid main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ intrepid-security main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ intrepid-updates main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ intrepid-proposed main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ intrepid-backports main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ intrepid main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ intrepid-security main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ intrepid-updates main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ intrepid-proposed main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ intrepid-backports main restricted universe multiverse
