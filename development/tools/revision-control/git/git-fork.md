git fork
========

http://help.github.com/fork-a-repo/ 

概要： 克隆别人的代码库到自己的项目中，可以作为子模块的形式使用，或二次开发 

操作流程： 

在开源项目中点击fork按钮，稍等一会儿，该项目便会拷贝一份到你的respositories中， 

克隆一份代码到本地：git clone git@github.com:username/Spoon-Knife.git 

配置：（项目克隆完成后，默认远程的别名为origin，此为我们自己项目中的版本，并非原始作者的代码库） 

创建原始代码库的别名，方便跟踪代码 git remote add upstream git://github.com/octocat/Spoon-Knife.git 

git fetch upstream 跟踪原始代码 

提交代码更新到自己的代码库 git push origin master 

获取原始代码库的更新 

git fetch upstream 

git merge upstream/master 

如果你希望将自己的代码贡献到原始代码库中，可参见http://help.github.com/send-pull-requests/ 来完成


