Vim扩展之NERDTree
================


NERDTree是VIM的扩展，听名字是：极客树。是用来做目录浏览的插件。

常用命令如下：

    :NERDTree /home/marty/vim7/src
    :NERDTree foo (foo is the name of a bookmark)
    以某目录或者书签为根打开一棵NERDTree

    :NERDTreeFromBookmark foo
    以书签为根打开一棵NERDTree

    :NERDTreeToggle [ | ]
    以某目录或者书签为根，切换NERDTree的状态，如果处于打开状态则关闭，如果处于关闭状态则打开

    :NERDTreeMirror
    在其他tab页，打开其他tab页的一个NERDTree镜像，如果有多个，会提示选择

    :NERDTreeClose
    关闭NERDTree

    :NERDTreeFind
    找到当前文件在NERDTree中的位置。如果当前没有打开的NERDTree，并且当前打开的文件在VIM CWD中，则初始化一棵NERDTree，并且显示当前文件的位置，否则，以该文件所在的文件夹为根生成一棵NERDTree。

    :NERDTreeCWD
    切换NERDTree的根为当前目录，如果当前没有NERDTree，则生成一棵。

    :Bookmark
    将当前节点，打上一个标签。

    :BookmarkToRoot
    以某标签为根，打开一棵NERDTree。

    :OpenBookmark
    打开某个标签，节点为文件在窗口打开，为文件夹则显示子节点。

    :ClearBookmarks []
    删除某个书签

    :ClearAllBookmarks
    删除所有标签

    :ReadBookmarks
    重新从NERDTreeBookmarksFile文件中读取标签。

快捷命令：

* o 在之前的窗口打开文件，文件夹，书签
* go 打开所选的文件，文件夹，书签，但是光标停留在NERDTree
* t 在新的tab打开文件，文件夹，书签
* T 打开所选的文件，文件夹，书签，但是光标停留在NERDTree
* i 以分屏方式打开，文件，文件夹，书签
* gi 以分屏方式打开，文件，文件夹，书签，但是光标停留在NERDTree
* s 水平分屏打开文件，
* gs 水平分别打开文件，但是光标停留在NERDTree
* O 打开所选的文件夹的所有嵌套文件夹
* x 关闭当前节点的父节点
* X 递归关闭所有当前节点，及其子节点
* e 编辑当前目录
* 同o
* double-click 同i
* middle-click 文件同i，文件夹同e
* D 删除所选标签
* P 跳到根节点
* p 跳到当前节点的父节点
* K 以当前节点深度，向上遍历节点
* J 以当前节点深度，向下遍历节点
* 向下跳转到当前目录的，兄弟节点
* 向上跳转到当前目录的，兄弟节点
* C 切换根节点到所选节点
* u 将根节点上移到上级文件夹
* U 同u，但是保持旧根节点打开状态
* r 递归刷新当前节点，子节点
* R 递归刷新根节点，子节点
* m 显示NERDTree菜单
* cd 切换CWD为当前选中节点
* CD 切换根节点为CWD
* I 切换显示隐藏文件
* f 切换文件过滤器
* F 切换显示文件（只显示文件夹）
* B 切换显示书签
* q 关闭NERDTree窗口
* A 最大，最小化NERDTree窗口
* ? 显示快捷命令帮助

配置项：

'loaded_nerd_tree' Turns off the script.

'NERDChristmasTree' Tells the NERD tree to make itself colourful
and pretty.

'NERDTreeAutoCenter' Controls whether the NERD tree window centers
when the cursor moves within a specified
distance to the top/bottom of the window.

'NERDTreeAutoCenterThreshold' Controls the sensitivity of autocentering.

'NERDTreeCaseSensitiveSort' Tells the NERD tree whether to be case
sensitive or not when sorting nodes.

'NERDTreeChDirMode' Tells the NERD tree if/when it should change
vim's current working directory.

'NERDTreeHighlightCursorline' Tell the NERD tree whether to highlight the
current cursor line.
'NERDTreeHijackNetrw' Tell the NERD tree whether to replace the netrw
autocommands for exploring local directories.

'NERDTreeIgnore' Tells the NERD tree which files to ignore.

'NERDTreeBookmarksFile' Where the bookmarks are stored.

'NERDTreeMouseMode' Tells the NERD tree how to handle mouse
clicks.

'NERDTreeQuitOnOpen' Closes the tree window after opening a file.

'NERDTreeShowBookmarks' Tells the NERD tree whether to display the
bookmarks table on startup.

'NERDTreeShowFiles' Tells the NERD tree whether to display files
in the tree on startup.

'NERDTreeShowHidden' Tells the NERD tree whether to display hidden
files on startup.

'NERDTreeShowLineNumbers' Tells the NERD tree whether to display line
numbers in the tree window.

'NERDTreeSortOrder' Tell the NERD tree how to sort the nodes in
the tree.

'NERDTreeStatusline' Set a statusline for NERD tree windows.

'NERDTreeWinPos' Tells the script where to put the NERD tree
window.

'NERDTreeWinSize' Sets the window size when the NERD tree is
opened.

'NERDTreeMinimalUI' Disables display of the 'Bookmarks' label and
'Press ? for help' text.

'NERDTreeDirArrows' Tells the NERD tree to use arrows instead of
+ ~ chars when displaying directories.

'NERDTreeCasadeOpenSingleChildDir'
Casade open while selected directory has only
one child that also is a directory.

'NERDTreeAutoDeleteBuffer' Tells the NERD tree to automatically remove
a buffer when a file is being deleted or renamed
via a context menu command.
