Mac OS X 程序员利器 – Homebrew安装与使用
========================================

##Homebrew安装与使用

### 什么是Homebrew？

`Homebrew is the easiest and most flexible way to install the UNIX tools Apple didn’t include with OS X.`

我们能够通过终端方便的使用它安装管理苹果没有自带的UNIX相关工具软件。

### 如何安装？

参考GitHub地址： https://github.com/mxcl/homebrew/wiki/installation

建议安装之前删除已经安装的Fink和Macports。

Homebrew使用ruby脚本，Mac OS X已经自带ruby。

    ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"

### 如何使用？

更新 formulae，保证每次安装的文件源是最新的。

    brew update

查看寻找自己需要的软件，例如想寻找jpeg相关的软件。

    brew search jpeg

出来结果有三个

    jpeg jpegoptim openjpeg

然后安装自己需要的软件

    brew install jpegoptim

Homebrew更新快速，操作简单安装方便，建议大家使用。如果formulae中的源代码下载地址被天朝墙掉，可以直接修改rb脚本中的源代码下载地址。
