比较Fink, macports 和 homebrew
==============================

如果你有Linux/Unix背景，那么在Mac上你一定想安装那些你熟悉的open source软件。 

Fink，Macports和Homebrew是3个主流的package management tool。 

### Fink 

Fink是基于Debian的packaging tools开发的。最大的特点是安装软件是预编译好的(pre-compiled/pre-built)。 

所以，用Fink安装package是不需要在本机编译的，都是现成的binary code。 

Fink最大的问题是package跟进不够快。很多最新版的软件，你要等Fink。 

### Macports 

Mac算是BSD的一个变种吧。所以，BSD的包管理软件port被移植到Mac上就显的理所当然了。 

macports的工作方式是下载source code然后在本地编译。macport的理念是尽量减少对系统现有库的依赖。 

所以，第一次用macport的时候，需要很长时间让macport重新build整个基本库，什么perl啊，python啊的。 

代价是较长的编译时间，较多的依赖关系下载。好处是不怎么依赖系统，也就是说，更新Mac OS不会破坏你现有的package。 

另外，macports安装所有的package到/opt/local下面。这样不会和系统现有的/usr/local有什么冲突。 

### Homebrew 

这个比较新，是在Lion之后才兴起的。 

工作方式和macport类似，也是下载source并在本地编译安装。但是和macports有两个根本的区别。 

* 1） homebrew的理念是尽量使用系统现有的库。这样可以大大的减少编译时间。 

* 2） package都安装到/usr/local下面。 

这两点和macports是完全相反的。结果也是有利有弊。 

最大的好处莫过于编译时间变短，安装简单。问题就是和系统紧密依赖。 

另外Homebrew假设你的Mac是单用户系统，所以/usr/local的owner应该是你，而不是传统的root。 

这个假设在大多数情况下都成立。 

（当然你可以改变homebrew的安装路径，然后修改你的PATH） 

另外，一个不太重要的区别，macport是用rsync来同步repository tree和获取新的package的。 

homebrew是用git来管理repository的。 

如果你是在内部网或者firewall后通过proxy使用，这点可能会给你带来影响--一些proxy不支持rsync的。 

macports可以用svn来代替rsync。虽然selfupdate不可用，但是其他的sync, install等完全不影响。这样就可以绕过上面所说的proxy的问题了。 

https://trac.macports.org/wiki/howto/SyncingWithSVN 

### 总结： 

1. 忘了Fink吧，老了。 

2. 如果你是重量级的Linux用户，希望使用所有的open source package，那么macports是你不二的选择。 

3. 如果你只希望很快的安装一些便利的工具，那么homebrew是个不错的选择。 

