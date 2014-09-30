ubuntu学习笔记
============

### apt-cache查找可用软件包

    apt-cache search {name}
    apt-cache depends ｛name｝//查找{name}依赖哪些包
    apt-get rdepends {name}  //查找哪些包依赖{name}

只有depends才能确定真正的依赖性，反向的依赖性不一定可靠。Depends开头的是必须安装的，Recommends不是必需的。如fping并不是mplayer是必需的。

另外apt-get install package时以Depends或是Recommends开头的行中的包都将被安装，而以Suggests开头的行中的包不会被安装。

---
