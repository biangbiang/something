# git log

git log 查看提交记录，参数：

`-n` (n是一个正整数)，查看最近n次的提交信息

    $ git log -2    查看最近2次的提交历史记录

`-- fileName` fileName为任意文件名，查看指定文件的提交信息。(注：文件名应该放到参数的最后位置，通常在前面加上--并用空格隔开表示是文件。)

    $ git log file1 file2   查看file1文件file2文件的提交记录
    $ git log file/         查看file文件夹下所有文件的提交记录

`--branchName` branchName为任意一个分支名字，查看莫个分支上的提交记录。同上，需要放到参数中的最后位置处。(注：如果分支名与文件名相同，系统会提示错误，可通过--选项来指定给定的参数是分支名还是文件名。)例：在当前分支中有一个名为v1的文件，同时还存在一个名为v1的分支,则：

    $ git log v1 -- 此时的v1代表的是分支名字
    $ git log -- v1 此时的v1代表的是名为v1的文件
    $ git log v1 -- v1

`tagName`或`branchame` 查询指定标签/分支中的提交记录信息

    $ git log v1.0..        查询从v1.0以后的提交历史记录(不包含v1.0)
    $ git log test..master  查询master分支中的提交记录但不包含test分支记录
    $ git log master..test  查询test分支中的提交记录但不办含master分支记录
    $ git log master...test 查询master或test分支中的提交记录。
    $ git log test --not master　　屏蔽master分支

根据commit查询日志    

    $ git log commit  　　查询commit之前的记录，包含commit
    $ git log commit1 commit2 查询commit1与commit2之间的记录，包括commit1和commit2
    $ git log commit1..commit2 同上，但是不包括commit1

其中，commit可以是提交哈希值的简写模式，也可以使用HEAD代替。HEAD代表最后一次提交，HEAD^为最后一个提交的父提交，等同于HEAD～1，HEAD～2代表倒数第二次提交

`--pretty` 按指定格式显示日志信息,可选项有：oneline,short,medium,full,fuller,email,raw以及format:<string>,默认为medium，可以通过修改配置文件来指定默认的方式。

    $ git log (--pretty=)oneline

常见的format选项：

    选项     说明
    %H      提交对象（commit）的完整哈希字串
    %h      提交对象的简短哈希字串
    %T      树对象（tree）的完整哈希字串
    %t      树对象的简短哈希字串
    %P      父对象（parent）的完整哈希字串
    %p      父对象的简短哈希字串
    %an     作者（author）的名字
    %ae     作者的电子邮件地址
    %ad     作者修订日期（可以用 -date= 选项定制格式）
    %ar     作者修订日期，按多久以前的方式显示
    %cn     提交者(committer)的名字
    %ce     提交者的电子邮件地址
    %cd     提交日期
    %cr     提交日期，按多久以前的方式显示
    %s      提交说明

注：作者是指最后一次修改文件的人；而提交者是指提交该文件的人。

    $ git log --pretty=format:"%an %ae %ad %cn %ce %cd %cr %s" --graph

`--mergs` 查看所有合并过的提交历史记录

`--no-merges` 查看所有未被合并过的提交信息

`--author=someonet` 查询指定作者的提交记录

    $ git log --author=gbyukg

`--since`，`--after` 仅显示指定时间之后的提交(不包含当前日期)

`--until`，`--before` 仅显示指定时间之前的提交(包含当前日期)

    $ git log --before={3,weeks,ago} --after={2010-04-18}

`--grep` 通过提交说明信息过滤提交日志

    $ git log --grep=hotfix 该命令会列出所有包含hotfix字样的提交信息说明的提交记录

注意：如果想同时使用--grep和--author，必须在附加一个--all-match参数。

`-S` 通过查询文件的变更内容来检索出指定提交日志 注：-S后没有"="，与查询内容之间也没有空格符

    $ git log --Snew

`-p` 查看提交时的补丁信息

    $ git log -p --no-merges -2

`--stat`  列出文件的修改行数

`--sortstat`      只显示--stat中最后行数修改添加移除的统计

`--graph` 以简单的图形方式列出提交记录

`--abbrev-commit` 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。

`--relative-date` 使用较短的相对时间显示（比如，“2 weeks ago”）。

`--name-only` 仅在提交信息后显示已修改的文件清单。

`--name-status` 显示新增、修改、删除的文件清单。

GIT Blame

用来查看文件的每个部分修改详情

    $git blame index.php
