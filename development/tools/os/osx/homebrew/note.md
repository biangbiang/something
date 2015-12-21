homebrew学习笔记
===============

### 国内镜像

Mac 终端跑入即可

    cd /usr/local/  
    git remote set-url origin http://mirrors.ustc.edu.cn/homebrew.git  
    或者这个：
    git remote set-url origin git://mirrors.tuna.tsinghua.edu.cn/homebrew.git  
    brew update  

如果还是感觉慢，看看这个：

    http://mirrors.tuna.tsinghua.edu.cn/help/#homebrew

---

### homebrew 常用命令

mac 系统常用的软件安装工具就是 homebrew, 其最常用的命令如下：

1. 安装（需要 Ruby）：

        ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

2. 搜索：brew search mysql

3. 查询：brew info mysql 主要看具体的信息，比如目前的版本，依赖，安装后注意事项等

4. 更新：brew update 这会更新 Homebrew 自己，并且使得接下来的两个操作有意义——

5. 检查过时（是否有新版本）：brew outdated 这回列出所有安装的软件里可以升级的那些

6. 升级：brew upgrade 升级所有可以升级的软件们

7. 清理：brew cleanup 清理不需要的版本极其安装包缓存

常用的就这些。一般来说如果你追求新版本（不升级不舒服斯基），那么你最常用的操作序列就是这样：

    brew update          # 更新 Homebrew 的信息
    brew outdated        # 看一下哪些软件可以升级
    brew upgrade <xxx>   # 如果不是所有的都要升级，那就这样升级指定的

    brew upgrade; brew cleanup    # 如果都要升级，直接升级完然后清理干净
