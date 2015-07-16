SVN trunk、branch、tag的用法
===========================

Subversion有一个很标准的目录结构，是这样的。

比如项目是proj，svn地址为svn://proj/，那么标准的svn布局是

    svn://proj/|+-trunk+-branches+-tags

这是一个标准的布局，trunk为主开发目录，branches为分支开发目录，tags为tag存档目录（不允许修改）。但是具体这几个目录应该如何使用，svn并没有明确的规范，更多的还是用户自己的习惯。

对于这几个开发目录，一般的使用方法有两种。我更多的是从软件产品的角度出发（比如freebsd），因为互联网的开发模式是完全不一样的。

### 第一种方法，使用trunk作为主要的开发目录。

一般的，我们的所有的开发都是基于trunk进行开发，当一个版本/release开发告一段落（开发、测试、文档、制作安装程序、打包等）结束后，代码处于冻结状态（人为规定，可以通过hook来进行管理）。此时应该基于当前冻结的代码库，打tag。当下一个版本/阶段的开发任务开始，继续在trunk进行开发。

此时，如果发现了上一个已发行版本（Released Version）有一些bug，或者一些很急迫的功能要求，而正在开发的版本（Developing Version）无法满足时间要求，这时候就需要在上一个版本上进行修改了。应该基于发行版对应的tag，做相应的分支（branch）进行开发。

例如，刚刚发布1.0，正在开发2.0，此时要在1.0的基础上进行bug修正。

按照时间的顺序

1.0开发完毕，代码冻结

基于已经冻结的trunk，为release1.0打tag

此时的目录结构为

    svn://proj/
                 +trunk/ (freeze)
                 +branches/
                 +tags/
                         +tag_release_1.0　(copy from trunk)

2.0开始开发，trunk此时为2.0的开发版

发现1.0有bug，需要修改，基于1.0的tag做branch

此时的目录结构为

    svn://proj/
                 +trunk/ ( dev 2.0 )
                 +branches/
                               +dev_1.0_bugfix (copy from tag/release_1.0)
                 +tags/
                         +release_1.0　(copy from trunk)

在1.0 bugfix branch进行1.0 bugfix开发，在trunk进行2.0开发

在1.0 bugfix 完成之后，基于dev_1.0_bugfix的branch做release等

根据需要选择性的把dev_1.0_bugfix这个分支merge回trunk（什么时候进行这步操作，要根据具体情况）

这是一种很标准的开发模式，很多的公司都是采用这种模式进行开发的。trunk永远是开发的主要目录。

### 第二种方法，在每一个release的branch中进行各自的开发，trunk只做发布使用。

这种开发模式当中，trunk是不承担具体开发任务的，一个版本/阶段的开发任务在开始的时候，根据已经release的版本做新的开发分支，并且基于这个分支进行开发。还是举上面的例子，这里面的时序关系是。

1.0开发，做dev1.0的branch

此时的目录结构

    svn://proj/
                 +trunk/ (不担负开发任务 )
                 +branches/
                               +dev_1.0 (copy from trunk)
                 +tags/

1.0开发完成，merge dev1.0到trunk

此时的目录结构

    svn://proj/
                 +trunk/ (merge from branch dev_1.0)
                 +branches/
                               +dev_1.0 (开发任务结束，freeze)
                 +tags/

根据trunk做1.0的tag

此时的目录结构

    svn://proj/
                 +trunk/ (merge from branch dev_1.0)
                 +branches/
                               +dev_1.0 (开发任务结束，freeze)
                 +tags/
                         +tag_release_1.0 (copy from trunk)

1.0开发，做dev2.0分支

此时的目录结构

    svn://proj/
                 +trunk/
                 +branches/
                               +dev_1.0 (开发任务结束，freeze)
                               +dev_2.0 （进行2.0开发）
                 +tags/
                         +tag_release_1.0 (copy from trunk)

1.0有bug，直接在dev1.0的分支上修复

---

### 主干(trunk)、分支(branch )、标记(tag)

在SVN中Branch/tag在一个功能选项中，在使用中也往往产生混淆。
 
在实现上，branch和tag，对于svn都是使用copy实现的，所以他们在默认的权限上和一般的目录没有区别。至于何时用tag，何时用branch，完全由人主观的根据规范和需要来选择，而不是强制的（比如cvs）。

一般情况下，

* trunk：是用来做主方向开发的，一个新模块的开发，这个时候就放在trunk，当模块开发完成后，需要修改，就用branch。
* branch：是用来做并行开发的，这里的并行是指和trunk进行比较。
* tag：是用来做一个milestone的，不管是不是发布版本，但都是一个可用的版本。这里，应该是只读的。更多的是一个显示用的，给人一个可读的标记。

比如，3.0开发完成，这个时候要做一个tag，tag_release_3_0，然后基于这个tag做发布，比如安装程序等。trunk进入 3.1的开发，但是3.0发现了bug，那么就需要基于tag_release_3_0做一个分支(branch)，branch_bugfix_3_0，基于这个branch进行bug修改，等到bugfix结束，做一个tag，tag_release_3_0_1，然后，根据需要决定 branch_bugfix_3_0是否并入主干(trunk)。

对于svn还要注意的一点，就是它是全局版本号，其实这个就是一个tag的标记，所以我们经常可以看到，什么什么release，基于xxx项目的 2xxxx版本。就是这个意思了。但是，它还明确的给出一个tag的概念，就是因为这个更加的可读，毕竟记住tag_release_1_0要比记住一个很大的版本号容易的多。

#### branches：分枝

当多个人合作，可能有这样的情况出现：John突然有个想法，跟原先的设计不太一致，可能是功能的添加或者日志格式的改进等等，总而言之，这个想法可能需要花一段时间来完成，而这个过程中，John的一些操作可能会影响Sally的工作，John从现有的状态单独出一个project的话，又不能及时得到 Sally对已有代码做的修正，而且独立出来的话，John的尝试成功时，跟原来的合并也存在困难。这时最好的实践方法是使用branches。 John建立一个自己的branch，然后在里面实验，必要的时候从Sally的trunk里取得更新，或者将自己的阶段成果汇集到trunk中。

创建分支的命令：

    (svn copy SourceURL/trunk DestinationURL/branchName -m "Creating a private branch of xxxx/trunk." )

#### trunk：主干

主干，一般来说就是开发的主要呆的地方，

#### tag:：标记

在经过了一段时间的开发后，项目到达了一个里程碑阶段，你可能想记录这一阶段的代码的状态，那么你就需要给代码打上标签。

创建标记的命令：

    (svn cp file:///svnroot/mojavescripts/trunk file:///svnroot/mojavescripts/tags/mirrorutils_rel_0_0_1-m "taged mirrorutils_rel_0_0_1")

另有一说，无所谓谁对谁错。

* trunk：表示开发时版本存放的目录，即在开发阶段的代码都提交到该目录上。
* branches：表示发布的版本存放的目录，即项目上线时发布的稳定版本存放在该目录中。
* tags：表示标签存放的目录。

在这需要说明下分三个目录的原因，如果项目分为一期、二期、三期等，那么一期上线时的稳定版本就应该在一期完成时将代码copy到branches上，这样二期开发的代码就对一期的代码没有影响，如新增的模块就不会部署到生产环境上。而branches上的稳定的版本就是发布到生产环境上的代码，如果用户使用的过程中发现有bug，则只要在branches上修改该bug，修改完bug后再编译branches上最新的代码发布到生产环境即可。
 
tags的作用是将在branches上修改的bug的代码合并到trunk上时创建个版本标识，以后branches上修改的bug代码再合并到trunk上时就从tags的version到branches最新的version合并到trunk，以保证前期修改的bug代码不会再合并。

---

一直以来用svn只是当作cvs，也从来没有仔细看过文档，直到今天用到，才去翻看svn book文档，惭愧

需求一：

有一个客户想对产品做定制，但是我们并不想修改原有的svn中trunk的代码。

方法：

用svn建立一个新的branches，从这个branche做为一个新的起点来开发

    svn copy svn://server/trunk svn://server/branches/ep -m "init ep"

Tip:

如果你的svn中以前没有branches这个的目录，只有trunk这个，你可以用

    svn mkdir branches

新建个目录

需求二：

产品开发已经基本完成，并且通过很严格的测
 
---

——简单的对比

SVN的工作机制在某种程度上就像一颗正在生长的树：

* 一颗有树干和许多分支的树
* 分支从树干生长出来，并且细的分支从相对较粗的树干中长出
* 一棵树可以只有树干没有分支（但是这种情况不会持续很久，随着树的成长，肯定会有分支啦，^^）
* 一颗没有树干但是有很多分支的树看起来更像是地板上的一捆树枝
* 如果树干患病了，最终分支也会受到影响，然后整棵树就会死亡
* 如果分支患病了，你可以剪掉它，然后其他分支还会生长出来的哦！
* 如果分支生长太快了，对于树干它可能会非常沉重，最后整棵树会垮塌掉
* 当你感觉你的树、树干或者是分支看起来很漂亮的时候，你可以给它照张相，这样就就可以记得它在那时是多么的赞。

——Trunk

Trunk是放置稳定代码的主要环境，就好像一个汽车工厂，负责将成品的汽车零件组装在一起。

以下内容将告诉你如何使用SVN trunk：

* 除非你必须处理一些容易且能迅速解决的BUG，或者你必须添加一些无关逻辑的文件（比如媒体文件：图像，视频，CSS等等），否则永远不要在trunk直接做开发
* 不要因为特殊的需求而去对先前的版本做太大的改变，如何相关的情况都意味着需要建立一个branch（如下所述）
* 不要提交一些可能破坏trunk的内容，例如从branch合并
* 如果你在某些时候偶然间破坏了trunk，bring some cake the next day (”with great responsibilities come… huge cakes”)

——Branches

一个branch就是从一个SVN仓库中的子树所作的一份普通拷贝。通常情况它的工作类似与UNIX系统上的符号链接，但是你一旦在一个SVN branch里修改了一些文件，并且这些被修改的文件从拷贝过来的源文件独立发展，就不能这么认为了。当一个branch完成了，并且认为它足够稳定的时候，它必须合并回它原来的拷贝的地方，也就是说：如果原来是从trunk中拷贝的，就应该回到trunk去，或者合并回它原来拷贝的父级branch。

以下内容将告诉你如何使用SVN branches：

* 如果你需要修改你的应用程序，或者为它开发一个新的特性，请从trunk中创建一个新的branch，然后基于这个新的分支进行开发
* 除非是因为必须从一个branch中创建一个新的子branch，否则新的branch必须从trunk创建
* 当你创建了一个新branch，你应当立即切换过去。如果你没有这么做，那你为什么要在最初的地方创建这个分支呢？

——Tags

从表面上看，SVN branches和SVN tags没有什么差别，但是从概念上来说，它们有许多差别。其实一个SVN tags就是上文所述的“为这棵树照张相”：一个trunk或者一个branch修订版的命名快照。

以下内容将告诉你如何使用SVN tags：

* 作为一个开发者，永远不要切换至、取出，或者向一个SVN tag提交任何内容：一个tag好比某种“照片”，并不是实实在在的东西，tags只可读，不可写。
* 在特殊或者需要特别注意的环境中，如：生产环境（production）、？（staging）、测试环境（testing）等等，只能从一个修复过的（fixed）tag中checkout和update，永远不要commit至一个tag。
* 对于上述提及到的环境，可以创建如下的tags：“production”，“staging”，“testing”等等。你也可以根据软件版本、项目的成熟程度来命名tag：“1.0.3”，“stable”，“latest”等等。
* 当trunk已经稳定，并且可以对外发布，也要相应地重新创建tags，然后再更新相关的环境（production, staging, etc）

——工作流样例

假设你必须添加了一个特性至一个项目，且这个项目是受版本控制的，你差不多需要完成如下几个步骤：

* 使用SVN checkout或者SVN switch从这个项目的trunk获得一个新的工作拷贝（branch）
* 使用SVN切换至新的branch
* 完成新特性的开发（当然，要做足够的测试，包括在开始编码前）
* 一旦这个特性完成并且稳定（已提交），并经过你的同事们确认，切换至trunk
* 合并你的分支至你的工作拷贝（trunk），并且解决一系列的冲突
* 重新检查合并后的代码
* 如果可能的话，麻烦你的同事对你所编写、更改的代码进行一次复查（review）
* 提交合并后的工作拷贝至trunk
* 如果某些部署需要特殊的环境（生成环境等等），请更新相关的tag至你刚刚提交到trunk的修订版本
* 使用SVN update部署至相关环境
