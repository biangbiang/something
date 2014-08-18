vim学习笔记
=========

### vim配置markdown语法高亮

发现md结尾的文件没有语法高亮，但是markdown结尾的文件有语法高亮。

尝试了 `:set filetype = markdown` 命令，md结尾的文件也高亮了。

所以果断放弃了装个markdown语法高亮插件的想法。

直接配置 `.vimrc`:

    au BufRead,BufNewFile *.{md,mdown,mkd,mkdn,markdown,mdwn} set filetype=mkd
