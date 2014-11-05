Composer
========

是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。

## PHP 开发者该知道的 5 个 Composer 小技巧

Composer 是新一代的PHP依赖管理工具。其介绍和基本用法可以看这篇《[Composer PHP依赖管理的新时代](http://www.phpcomposer.com/composer-the-new-age-of-dependency-manager-for-php)》。本文介绍使用Composer的五个小技巧，希望能给你的PHP开发带来方便。

### 1. 仅更新单个库

只想更新某个特定的库，不想更新它的所有依赖，很简单：

    composer update foo/bar  

此外，这个技巧还可以用来解决“警告信息问题”。你一定见过这样的警告信息：

    Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, run update to update them.  

擦，哪里出问题了？别惊慌！如果你编辑了composer.json，你应该会看到这样的信息。比如，如果你增加或更新了细节信息，比如库的描述、作者、更多参数，甚至仅仅增加了一个空格，都会改变文件的md5sum。然后Composer就会警告你哈希值和composer.lock中记载的不同。

那么我们该怎么办呢？update命令可以更新lock文件，但是如果仅仅增加了一些描述，应该是不打算更新任何库。这种情况下，只需update nothing：

    $ composer update nothing
    Loading composer repositories with package information  
    Updating dependencies  
    Nothing to install or update  
    Writing lock file  
    Generating autoload files  

这样一来，Composer不会更新库，但是会更新composer.lock。注意nothing并不是update命令的关键字。只是没有nothing 这个包导致的结果。如果你输入foobar，结果也一样。

如果你用的Composer版本足够新，那么你可以直接使用--lock选项：

    composer update --lock  

### 2. 不编辑composer.json的情况下安装库

你可能会觉得每安装一个库都需要修改composer.json太麻烦，那么你可以直接使用require命令。

    composer require "foo/bar:1.0.0"  

这个方法也可以用来快速地新开一个项目。init命令有--require选项，可以自动编写composer.json：（注意我们使用-n，这样就不用回答问题）

    $ composer init --require=foo/bar:1.0.0 -n
    $ cat composer.json
    {
        "require": {
            "foo/bar": "1.0.0"
        }
    }

### 3. 派生很容易

初始化的时候，你试过create-project命令么？

    composer create-project doctrine/orm path 2.2.0  

这会自动克隆仓库，并检出指定的版本。克隆库的时候用这个命令很方便，不需要搜寻原始的URI了。

### 4. 考虑缓存，dist包优先

最近一年以来的Composer会自动存档你下载的dist包。默认设置下，dist包用于加了tag的版本，例如"symfony/symfony": "v2.1.4"，或者是通配符或版本区间，"2.1.*"或">=2.2,<2.3-dev"（如果你使用stable作为你的minimum-stability）。

dist包也可以用于诸如dev-master之类的分支，Github允许你下载某个git引用的压缩包。为了强制使用压缩包，而不是克隆源代码，你可以使用install和update的--prefer-dist选项。

下面是一个例子（我使用了--profile选项来显示执行时间）：

    $ composer init --require="twig/twig:1.*" -n --profile
    Memory usage: 3.94MB (peak: 4.08MB), time: 0s

    $ composer install --profile
    Loading composer repositories with package information  
    Installing dependencies  
      - Installing twig/twig (v1.12.2)
        Downloading: 100%

    Writing lock file  
    Generating autoload files  
    Memory usage: 10.13MB (peak: 12.65MB), time: 4.71s

    $ rm -rf vendor

    $ composer install --profile
    Loading composer repositories with package information  
    Installing dependencies from lock file  
      - Installing twig/twig (v1.12.2)
        Loading from cache

    Generating autoload files  
    Memory usage: 4.96MB (peak: 5.57MB), time: 0.45s  

这里，twig/twig:1.12.2的压缩包被保存在~/.composer/cache/files/twig/twig/1.12.2.0-v1.12.2.zip。重新安装包时直接使用。

### 5. 若要修改，源代码优先

当你需要修改库的时候，克隆源代码就比下载包方便了。你可以使用--prefer-source来强制选择克隆源代码。

    composer update symfony/yaml --prefer-source  

接下来你可以修改文件：

    composer status -v  
    You have changes in the following dependencies:  
    /path/to/app/vendor/symfony/yaml/Symfony/Component/Yaml:
        M Dumper.php

当你试图更新一个修改过的库的时候，Composer会提醒你，询问是否放弃修改：

    $ composer update
    Loading composer repositories with package information  
    Updating dependencies  
      - Updating symfony/symfony v2.2.0 (v2.2.0- => v2.2.0)
        The package has modified files:
        M Dumper.php
        Discard changes [y,n,v,s,?]?

### 为生产环境作准备

最后提醒一下，在部署代码到生产环境的时候，别忘了优化一下自动加载：

composer dump-autoload --optimize  

安装包的时候可以同样使用--optimize-autoloader。不加这一选项，你可能会发现20%到25%的性能损失。

如果你需要帮助，或者想要了解某个命令的细节，你可以阅读[官方文档](http://getcomposer.org/)或者[中文文档](http://docs.phpcomposer.com/)，也可以查看JoliCode做的这个[交互式备忘单](http://composer.json.jolicode.com/)。

原文地址：[5 features to know about Composer PHP](http://moquet.net/blog/5-features-about-composer-php/) 

译文地址：[PHP 开发者该知道的 5 个 Composer 小技巧](http://segmentfault.com/a/1190000000355928)

---

## Composer PHP依赖管理的新时代

对于现代语言而言，包管理器基本上是标配。Java有Maven，Python有pip，Ruby有gem，Nodejs有npm。PHP的则是PEAR，不过PEAR坑不少：

* 依赖处理容易出问题
* 配置非常复杂
* 难用的命令行接口

好在我们有Composer，PHP依赖管理的利器。它是开源的，使用起来也很简单，提交自己的包也很容易。

### 安装Composer

Composer需要PHP 5.3.2+才能运行。

    $ curl -sS https://getcomposer.org/installer | php

这个命令会将composer.phar下载到当前目录。PHAR（PHP 压缩包）是一个压缩格式，可以在命令行下直接运行。

你可以使用--install-dir选项将Composer安装到指定的目录，例如：

    $ curl -sS https://getcomposer.org/installer | php -- --install-dir=bin

当然也可以进行全局安装：

    $ curl -sS https://getcomposer.org/installer | php
    $ mv composer.phar /usr/local/bin/composer

在Mac OS X下也可以使用homebrew安装：

    brew tap josegonzalez/homebrew-php  
    brew install josegonzalez/php/composer  

不过通常情况下只需将composer.phar的位置加入到PATH环境变量就可以，不一定要全局安装。

### 声明依赖

在项目目录下创建一个composer.json文件，指明依赖，比如，你的项目依赖 monolog：

    {
        "require": {
            "monolog/monolog": "1.2.*"
        }
    }

### 安装依赖

安装依赖非常简单，只需在项目目录下运行：

    composer install  

如果没有全局安装的话，则运行：

    php composer.phar install  

### 自动加载

Composer提供了自动加载的特性，只需在你的代码的初始化部分中加入下面一行：

    require 'vendor/autoload.php';  

### 模块仓库

packagist.org是Composer的仓库，很多著名的PHP库都能在其中找到。你也可以提交你自己的作品。

### 高级特性

以上介绍了Composer 的基本用法。Composer还有一些高级特性，虽然不是必需的，但是往往能给PHP开发带来方便。

项目主页

更多信息请访问 Composer 的官方主页或者中文站点。

---

## Composer 是什么

简单来说，Composer 是一个新的安装包管理工具，服务于 PHP 生态系统。它实际上包含了两个部分：Composer 和 Packagist.下面我们就简单说一下他们各自的用途。

### Composer

Composer 是由 Jordi Boggiano 和 Nils Aderman 创造的一个命令行工具，它的使命就是帮你为项目自动安装所依赖的开发包。Composer 中的很多理念都借鉴了 npm 和 Bundler，如果你对这两个工具有所了解的话，就会在 composer 中发现他们的身影。Composer 包含了一个依赖解析器，用来处理开发包之间复杂的依赖关系；另外，它还包含了下载器、安装器等有趣的东西。

作为一个用户，你所要做的就是在 composer.json 文件中声明当前项目所依赖的开发包，然后运行 composer.phar install 就行了。composer.json 文件定义了当前项目所依赖的开发包和 composer 的配置信息。下面是一个小型实例：

    {
        "require": {
            "monolog/monolog": "1.2.*"
        }
    }

### Packagist

Packagist 是 Composer 的默认的开发包仓库。你可以将自己的安装包提交到 packagist，将来你在自己的 VCS （源码管理软件，比如 Github） 仓库中新建了 tag 或更新了代码，packagist 都会自动构建一个新的开发包。这就是 packagist 目前的运作方式，将来 packagist 将允许直接上传开发包。
