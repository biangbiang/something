# 神奇的git stash

开发人员常常遇到这种情况：花了几天时间一直在做一个新功能，已经改了差不多十几个文件，突然有一个bug需要紧急解决，然后给一个build测试组。在Git问世之前基本上靠手动备份，费时且容易出错。

`git stash`命令简而言之就是帮助开发人员暂时搁置当前已做的改动，倒退到改动前的状态，进行其他的必要操作（比如发布，或者解决一个bug，或者branch，等等），之后还可以重新载入之前搁置的改动，很cool吧？

首先，用`git add`把所有的改动加到staging area。

    git add .

接着用git stash把这些改动搁置。

    git stash

到这里，当前工作平台就回复到改动之前了。该干嘛干嘛，此处省略1万字。

需要找回之前搁置的改动继续先前的工作了？

    git stash apply 

即可。

也可以用 `git stash list` 来查看所有的搁置版本（可能搁置了很多次，最好不要这样，容易搞混）

在出现一个搁置栈的情况下，比如如果你想找回栈中的第2个，可以用 `git stash apply stash@{1}`

如果想找回第1个，可以用 `git stash pop`

如果想删除一个stash，`git stash drop <id>`

删除所有stash，`git stash clear`
