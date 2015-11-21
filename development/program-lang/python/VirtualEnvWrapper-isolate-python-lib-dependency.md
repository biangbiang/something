使用VitrualEnvWrapper隔离python项目的库依赖
===========================================

### 是什么

VirtualEnv用于在一台机器上创建多个独立的python运行环境，VirtualEnvWrapper为前者提供了一些便利的命令行上的封装。

### 为什么要用

- 隔离项目之间的第三方包依赖，如A项目依赖django1.2.5，B项目依赖django1.3。
- 为部署应用提供方便，把开发环境的虚拟环境打包到生产环境即可,不需要在服务器上再折腾一翻。

### 怎么用

#### 安装

- pip install virtualenvwrapper
- 把下面这句加到~/.bash_profile里面，如不嫌麻烦，也可以每次都手动执行。

        source /usr/local/bin/virtualenvwrapper.sh

#### 常用命令

##### 创新的虚拟环境

- mkvirtualenv [env1]

该命令会帮我们创建一个新环境,默认情况下，环境的目录是.virtualenv/en1,创建过程中它会自动帮我们安装pip，以后我们要安装新依赖时可直接使用pip命令。

创建完之后，自动切换到该环境下工作，可看到提示符变为：

    (env1)$

在这个环境下安装的依赖不会影响到其他的环境

- lssitepackages 显示该环境中所安装的包

##### 切换环境

- workon [env]

随时使用“workon 环境名”可以进行环境切换，如果不带环境名参数，则显示当前使用的环境

- deactivate

在某个环境中使用，切换到系统的python环境

##### 其他命令

- showvirtualenv [env] 显示指定环境的详情。
- rmvirtualenv [env] 移除指定的虚拟环境，移除的前提是当前没有在该环境中工作。如在该环境工作，先使用deactivate退出。
- cpvirtualenv [source] [dest] 复制一份虚拟环境。
- cdvirtualenv [subdir] 把当前工作目录设置为所在的环境目录。
- cdsitepackages [subdir] 把当前工作目录设置为所在环境的sitepackages路径。
- add2virtualenv [dir] [dir] 把指定的目录加入当前使用的环境的path中，这常使用于在多个project里面同时使用一个较大的库的情况。
- toggleglobalsitepackages -q 控制当前的环境是否使用全局的sitepackages目录。
