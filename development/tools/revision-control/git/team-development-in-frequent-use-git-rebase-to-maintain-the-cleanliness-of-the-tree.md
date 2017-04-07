# 团队开发里频繁使用 git rebase 来保持树的整洁好吗

### 提问

用了以后, 树可以非常清晰, 某种程度上便于追踪, 但是 push --force 就多多了,

不用呢, 合并没有远程仓库被修改的麻烦, 可是追踪又不清晰...

怎样取舍? 团队里一般怎么取舍?

### 回答

git rebase是对commit history的改写。当你要改写的commit history还没有被提交到远程repo的时候，也就是说，还没有与他人共享之前，commit history是你私人所有的，那么想怎么改写都可以。

而一旦被提交到远程后，这时如果再改写history，那么势必和他人的history长的就不一样了。git push的时候，git会比较commit history，如果不一致，commit动作会被拒绝，唯一的办法就是带上-f参数，强制要求commit，这时git会以committer的history覆写远程repo，从而完成代码的提交。虽然代码提交上去了，但是这样可能会造成别人工作成果的丢失，所以使用-f参数要慎重。

楼主遇到的问题，就是改写了公有的commit history造成的。要解决这个问题，就要从提交流程上做规范。

举个正确流程的栗子：

假设楼主的team中有两个developer：tom和jerry，他们共同使用一个远程repo，并各自clone到自己的机器上，为了简化描述，这里假设只有一个branch：master。

这时tom机器的repo有两个branch

    master, origin/master

而jerry的机器上也是有两个branch

    master, origin/master

均如下图所示

![](http://biangbiangpic.b0.upaiyun.com/blog/3360572f9beaf4f91364f748c28fd050.png)

tom和jerry分别各自开发自己的新feature，不断有新的commit提交到他们各自私有的commit history中，所以他们的master指针不断的向前推移，分别指向不同的commit。而又由于他们都没有git fetch和git push，所以他们的origin/master都维持不变。

jerry的repo如下

![](http://biangbiangpic.b0.upaiyun.com/blog/c4f3a440a847dbfc28763002df6f5d41.png)

tom的repo如下，注意T1和上图的J1，分别是两个不同的commit

![](http://biangbiangpic.b0.upaiyun.com/blog/86539d0a47fdbaf2577049b61a998ec5.png)

这时Tom首先把他的commit提交的远程repo中，那么他本机origin/master指针则会前进，和master指针保持一致，如下

![](http://biangbiangpic.b0.upaiyun.com/blog/c5143f52311f7f2e29ad0d30e25e7c44.png)

远程repo如下

![](http://biangbiangpic.b0.upaiyun.com/blog/0c90221656803fbb5db3fb9362021260.png)

现在jerry也想把他的commit提交到远程repo上去，运行git push，毫无意外的失败了，所以他git fetch了一下，把远程repo，也就是之前tom提交的T1给拉到了他本机repo中，如下

![](http://biangbiangpic.b0.upaiyun.com/blog/51da95034b278843435cd7a5dabc5a2e.png)

commit history出现了分叉，要想把tom之前提交的内容包含到自己的工作中来，有一个方法就是git merge，它会自动生成一个commit，既包含tom的提交，也包含jerry的提交，这样就把两个分叉的commit重新又合并在一起。但是这个自动生成的commit会有两个parent，review代码的时候必须要比较两次，很不方便。

jerry为了保证commit history的线性，决定采用另外一种方法，就是git rebase。jerry的提交J1这时还没有被提交到远程repo上去，也就是他完全私有的一个commit，所以使用git rebase改写J1的history完全没有问题，改写之后，如下

![](http://biangbiangpic.b0.upaiyun.com/blog/9427c5ee1f28b5b7d682d682699aac24.png)

注意J1被改写到T1后面了，变成了J1`

git push后，本机repo

![](http://biangbiangpic.b0.upaiyun.com/blog/5cee9516538ef78240f3ffc38155972c.png)

而远程repo

![](http://biangbiangpic.b0.upaiyun.com/blog/554517662abffb693d2808ed29273576.png)

异常的轻松，一条直线，没有-f

所以，在不用-f的前提下，想维持树的整洁，方法就是：在git push之前，先git fetch，再git rebase。

    git fetch origin master
    git rebase origin/master
    git push

强烈推荐阅读

[a successful git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

[Git-分支-分支的衍合](https://git-scm.com/book/zh/v1/Git-分支-分支的衍合)
