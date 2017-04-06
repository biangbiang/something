# git 如何回滚远程仓库版本

前言： 使用git管理项目开发的过程中经常会碰到这种情况：某次提交已经push到了远程仓库，可是突然需要回退代码，怎么将远程代码库回滚呢？

不推荐这样做：在网上看到大部分人给出的解决方案是先将本地回滚，然后删除远程分支，之后再将本地的分支push到远程仓库，这其实是一种很危险的方案，毕竟直接删除远程分支太危险。

### 关于远程仓库回滚

首先，必须要明白的一件事，任何普通用户不能擅自做有关远程仓库回退的操作，如果你擅自回滚了远程仓库，会对项目团队其他人造成不可预知的影响。如果需要回退版本，先联系项目的仓库管理员，在团队其他人都对自己本地未提交的工作做好备份之后，再进行远程仓库回退操作，操作结束后，团队成员需要重新同步远程仓库后继续自己的工作。

通常回滚远程仓库会有以下三种情形：

##### 1、删除最后一次提交

这种情况是最简单的了，只需要以下两步就可以了

    git revert HEAD
    git push origin master

注意，revert和reset的区别:

revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在，而reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。如果还没有理解的话，我们做如下测试：

假设我们有以下三次提交记录：

![](http://biangbiangpic.b0.upaiyun.com/blog/762e6388feadec3570ba70f428f93ecc.png)

现在我们使用revert放弃最后一次提交，之后执行git log：

    git revert HEAD
    git log

![](http://biangbiangpic.b0.upaiyun.com/blog/4679909b5cdc0dbffb32ce6f6a85c08c.png)

历史记录中还有第三次提交的记录，并且多了一次的提交，但是仓库内容已经回到了第二次提交之后的状态。 现在我们使用reset回到第三次提交，之后执行git log：

    git reset --hard HEAD^
    git log

![](http://biangbiangpic.b0.upaiyun.com/blog/35922cbd23586057852e1406a8d46703.png)

历史记录中已经没有之前revert生成的提交记录了，现在应该明白了吧。 如果删除远程仓库的最后一次提交的时候不需要保留历史记录的话，可以使用reset，命令如下：

    git reset --hard HEAD^
    git push origin master -f

-f 参数是强制提交，因为reset之后本地库落后于远程库一个版本，因此需要强制提交。

##### 2、删除历史某次提交

这种情况需要先用git log命令在历史记录中查找到想要删除的某次提交的commit id，比如下图中圈出来的就是注释为"2"的提交的commit id（由此可见提交的注释很重要，一定要认真写）

![](http://biangbiangpic.b0.upaiyun.com/blog/6c3b8224afe0eadea297c7bbbeea7170.png)

然后执行以下命令（"commit id"替换为想要删除的提交的"commit id"，需要注意最后的^号，意思是commit id的前一次提交）：

    git rebase -i "commit id"^

执行该条命令之后会打开一个编辑框，内容如下，列出了包含该次提交在内之后的所有提交。

![](http://biangbiangpic.b0.upaiyun.com/blog/a01ba6aa0e1f6bc9c6025479de86761c.png)

然后在编辑框中删除你想要删除的提交所在行，然后保存退出就好啦，如果有冲突的需要解决冲突。接下来，执行以下命令，将本地仓库提交到远程库就完成了：

    git push origin master -f

##### 3、修改历史某次提交

这种情况的解决方法类似于第二种情况，只需要在第二条打开编辑框之后，将你想要修改的提交所在行的pick替换成edit然后保存退出，这个时候rebase会停在你要修改的提交，然后做你需要的修改，修改完毕之后，执行以下命令：

    git add .
    git commit --amend
    git rebase --continue

如果你在之前的编辑框修改了n行，也就是说要对n次提交做修改，则需要重复执行以上步骤n次。

需要注意的是，在执行rebase命令对指定提交修改或删除之后，该次提交之后的所有提交的"commit id"都会改变。
