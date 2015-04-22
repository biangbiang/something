svn学习笔记
===========

### 将文件加入忽略列表

1.先设置SVN默认的编辑器

    export SVN_EDITOR=vim

2.使用svn propedit svn:ignore ,用法如下

    svn propedit svn:ignore {your fold name}
