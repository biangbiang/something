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

### Git 获取远程分支

另一哥们将分支push到库中，我怎么获取到他的分支信息呢？

如果安装了git客户端，直接选择fetch一下，就可以获取到了。

如果用命令行，运行 git fetch，可以将远程分支信息获取到本地，再运行 git checkout -b local-branchname origin/remote_branchname  就可以将远程分支映射到本地命名为local-branchname  的一分支。 

### 如何在github上发起一个pull request

有一个仓库，叫Repo A。你如果要往里贡献代码，首先要Fork这个Repo，于是在你的Github账号下有了一个Repo A2,。然后你在这个A2下工作，Commit，push等。然后你希望原始仓库Repo A合并你的工作，你可以在Github上发起一个Pull Request，意思是请求Repo A的所有者从你的A2合并分支。如果被审核通过并正式合并，这样你就为项目A做贡献了
