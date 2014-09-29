git 自动补全
============

Git 通过 bash-completion 软件包实现命令自动补齐，在 Mac OS X 下可以通过 Homebrew 进行安装。

	$ brew search completion  
	bash-completion  
	$ brew install bash-completion  
	...  
	Add the following lines to your ~/.bash_profile file:  
	if [ -f 'brew --prefix'/etc/bash_completion ]; then  
	 . 'brew --prefix'/etc/bash_completion  
	fi  
	... 

根据 bash-completion 在安装过程中的提示，修改配置文件 ~/.bash_profile和~/.bashrc，并在其中加入如下内容，以便在终端加载时自动启用命令补齐。

	if [ -f 'brew --prefix'/etc/bash_completion ]; then  
	 . 'brew --prefix'/etc/bash_completion  
	fi 

将 Git 的命令补齐脚本拷贝到 bash-completion 对应的目录中。

	$ cp contrib/completion/git-completion.bash 'brew --prefix'/etc/bash_completion.d/ 

不用重启终端程序，只需要运行下面的命令即可立即在当前的 shell 中加载命令补齐。

	. 'brew --prefix'/etc/bash_completion 
