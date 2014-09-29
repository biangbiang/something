Ruby Gem命令详解
==============

### Gem介绍：

Gem是一个管理Ruby库和程序的标准包，它通过Ruby Gem（如 http://rubygems.org/ ）源来查找、安装、升级和卸载软件包，非常的便捷。

Ruby 1.9.2版本默认已安装Ruby Gem，如果你使用其它发行版本，请参考“如何安装Ruby Gem”。



### Ruby gem包的安装方式：

所有的gem包，会被安装到 /[Ruby root]/lib/ruby/gems/[ver]/ 目录下，这其中包括了Cache、doc、gems、specifications 4个目录，cache下放置下载的原生gem包，gems下则放置的是解压过的gem包。
当安装过程中遇到问题时，可以进入这些目录，手动删除有问题的gem包，然后重新运行 gem install [gemname] 命令即可。

### Ruby Gem命令详解：

    # 更新Gem自身
    # 注意：在某些linux发行版中为了系统稳定性此命令禁止执行
    $ gem update --system
    
    # 从Gem源安装gem包
    $ gem install [gemname]
    
    # 从本机安装gem包
    $ gem install -l [gemname].gem
    
    # 安装指定版本的gem包
    $ gem install [gemname] --version=[ver]
    
    # 更新所有已安装的gem包
    $ gem update
    
    # 更新指定的gem包
    # 注意：gem update [gemname]不会升级旧版本的包，此时你可以使用 gem install [gemname] --version=[ver]代替
    $ gem update [gemname]
    
    # 删除指定的gem包，注意此命令将删除所有已安装的版本
    $ gem uninstall [gemname]
    
    # 删除某指定版本gem
    $ gem uninstall [gemname] --version=[ver]
    
    # 查看本机已安装的所有gem包
    $ gem list [--local]
