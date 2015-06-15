mac删除pkg
==========

mac osx上大多数应用程序都是通过.DMG或者.pkg来安装的（当然brew方式安装的除外）。

如果是通过DMG方式安装的软件，要删除它就挺简单，直接从osx的应用程序文件夹上删除即可。

那么pkg方式安装的软件又该怎么删除呢？

mac并没有提供什么快捷删除方式，我们只能找到pkg安装后产生的文件，然后删除它。

### 方式一

mac会维护一份pkg安装历史，只要找到那个文件夹，我们就可以对症下药了。

我的机子是osx 10.8.4，pkg历史安装列表在/private/var/db/receipts目录下：

    cd /private/var/db/receipts
    ls -l

列出该目录，可以看到类似如下内容：

    -rw-r--r--    1 root  wheel     47315  6 15 15:48 com.codeius.izip.bom
    -rw-r--r--    1 root  wheel       253  6 15 15:48 com.codeius.izip.plist

找到.bom文件后，我们就可以使用以下命令找到安装的文件列表：

    lsbom -pf com.codeius.izip.bom

执行上述命令后，输出了类似以下内容：

    /iZip.app
    ./iZip.app/Contents
    ./iZip.app/Contents/CodeResources
    ./iZip.app/Contents/Frameworks
    ./iZip.app/Contents/Frameworks/libarchive.2.dylib
    ./iZip.app/Contents/Frameworks/libcurl.framework
    ./iZip.app/Contents/Frameworks/libcurl.framework/Headers
    ./iZip.app/Contents/Frameworks/libcurl.framework/Resources
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/CodeResources
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curl.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild32.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild64.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlrules.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlver.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/easy.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/mprintf.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/multi.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/stdcheaders.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/typecheck-gcc.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/types.h
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Resources
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Resources/Info.plist
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/_CodeSignature
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/_CodeSignature/CodeResources
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/libcurl
    ./iZip.app/Contents/Frameworks/libcurl.framework/Versions/Current
    ./iZip.app/Contents/Frameworks/libcurl.framework/libcurl
    ./iZip.app/Contents/Info.plist
    ./iZip.app/Contents/MacOS
    ./iZip.app/Contents/MacOS/iZip
    ./iZip.app/Contents/PkgInfo
    ./iZip.app/Contents/Resources
    ...

以上找到的这些文件，就是安装pkg的时候产生的文件，这些文件删除了应该就ok了！

### 方式二

方式一虽然可行，但是很多人都抱怨没找到pkg安装历史列表目录（确实不同版本的系统，目录是不一样的），那有没有更自动一点的呢？

pkgutil命令这个时候就派上用场了。查看一下帮助：

    man pkgutil # 或者直接pkgutil

看看有啥功能我们能用的:

    Receipt Database Commands:
      --pkgs, --packages     List all currently installed package IDs on --volume
      --pkgs-plist           List all package IDs on --volume in plist format
      ...
      --files PKGID          List files installed by the specified package
      ...

我们先找一下我们要删除的pkg包名（以上述iZip为例）：

    pkgutil --pkgs | grep -i izip

看一下输出了啥：

    com.codeius.izip

ok，现在就可以找出izip pkg到底装了哪些文件：

    pkgutil --file com.codeius.izip

终端输出的结果：

    iZip.app
    iZip.app/Contents
    iZip.app/Contents/CodeResources
    iZip.app/Contents/Frameworks
    iZip.app/Contents/Frameworks/libarchive.2.dylib
    iZip.app/Contents/Frameworks/libcurl.framework
    iZip.app/Contents/Frameworks/libcurl.framework/Headers
    iZip.app/Contents/Frameworks/libcurl.framework/Resources
    iZip.app/Contents/Frameworks/libcurl.framework/Versions
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/CodeResources
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curl.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild32.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlbuild64.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlrules.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/curlver.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/easy.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/mprintf.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/multi.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/stdcheaders.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/typecheck-gcc.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Headers/curl/types.h
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Resources
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/Resources/Info.plist
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/_CodeSignature
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/_CodeSignature/CodeResources
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/A/libcurl
    iZip.app/Contents/Frameworks/libcurl.framework/Versions/Current
    iZip.app/Contents/Frameworks/libcurl.framework/libcurl
    iZip.app/Contents/Info.plist
    iZip.app/Contents/MacOS
    iZip.app/Contents/MacOS/iZip
    iZip.app/Contents/PkgInfo
    iZip.app/Contents/Resources
    ...

把这些文件删了应该就算卸载完毕。（risk on your own!）

### 方式三

哈哈，有的人要讲究效率，不喜欢折腾半天，就会问能不能一键搞定啊？

真别说，还真有http://www.corecode.at/uninstallpkg/，偶然发现这个东西，貌似不错，试用了一下，好像真成功卸载了。（就是不知道有没有卸载干净...）
