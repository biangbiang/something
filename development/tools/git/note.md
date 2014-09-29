git学习笔记
=========

### git submodule

簡單來說，Git Submodule 可以輕易地將別人的 git 掛入到你目前 git 的任何位置。

### git blame

如果你要查看文件的每个部分是谁修改的, 那么 git blame 就是不二选择. 只要运行'git blame [filename]', 你就会得到整个文件的每一行的详细修改信息:包括SHA串,日期和作者

* git blame -w # 忽略移除空白这类改动
* git blame -M # 忽略移动文本内容这类改动
* git blame -C # 忽略移动文本内容到其它文件这类改动

### git fsck

    git fsck –lost-found

### git 删除远程分支

一不小心把本地的临时分支push到server上去了，想要删除。
一开始用

    git branch -r -d origin/branch-name

不成功，发现只是删除的本地对该远程分支的track，正确的方法应该是这样：

    git push origin :branch-name

冒号前面的空格不能少，原理是把一个空分支push到server上，相当于删除该分支。
