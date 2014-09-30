dpkg-reconfigure命令
===================

### 1.功能作用

重新配制一个已经安装的软件包

当用户需要再次对软件包配置的时候,可以使用dpkg-reconfigure来对指定的软件包进行配置.

### 2.位置

/usr/bin/debconf-set-selections

### 3.格式用法

	dpkg-reconfigure [选项] 软件包

### 4.主要参数

	-a,  --all            重配置所有软件包。
	-u,  --unseen-only        仅显示未提过的问题。
	--default-priority    使用默认优先级，而非“低”级。
	--force            强迫重配置受损软件包。
	--no-reload        不要轻易的重装模板(使用时请慎重考虑)。
	-f,  --frontend        指定 debconf 前端界面。
	-p,  --priority        指定要显示的问题的最优先级。
	--terse            开启简要模式。

### 5.应用实例

1、用于配置语言

	sudo dpkg-reconfigure locales
