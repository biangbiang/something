VIM编写markdown文档
==================

### 目录结构

	$ tree -C ~/.vim | grep -v ".cnx"

	|-- syntax
	    |-- mkd.vim

### markdown 简介

[markdown](http://zh.wikipedia.org/wiki/Markdown) 是一种轻量级标记语言，允许人们"使用易读易写的纯文本格式编写文档， 然后转换成有效的XHTML(或者HTML)文档"。 – 引用自维基百科

### 安装语法文件

	$ cd ~/.vim/syntax/
	$ wget https://raw.github.com/plasticboy/vim-markdown/master/syntax/mkd.vim

此语法文件用来高亮 markdown 文本，而 markdown 的语法请参考 [中文版](https://github.com/riku/Markdown-Syntax-CN/blob/master/syntax.md)，[英文版](http://daringfireball.net/projects/markdown/syntax)。

### 预览

markdown 文件可以使用 chrome 的 markdown preview 插件进行预览， 这里的预览实际上是把 .md / .markdown / .mkd 结尾的文件在 chrome 中打开自动转换为 html 格式显示。

### markdown 转换为 html

下面列出 3 种我用过的转换方式供大家参考：

一. perl 官方提供了使用 [Markdown.pl](http://daringfireball.net/projects/downloads/Markdown_1.0.1.zip) 文件进行转换。 使用方法（结合了vim）：

	$ unzip Markdown_1.0.1.zip
	$ sudo cp Markdown_1.0.1/Markdown.pl /usr/local/bin/
	$ vi ~/.vimrc
	" 添加如下代码
	let mapleader = ","
	nnoremap <leader>md :%!/usr/local/bin/Markdown.pl --html4tags <CR>

此时在 VIM 普通模式下输入 ,md 就可以将 markdown 文本转换为 html 格式。 如果不想通过 VIM，可以直接通过 /path/to/Markdown.pl --html4tags markdown.text > markdown.html， 将 markdown.text 文本转成 html 格式后输出到 markdown.html 文件

二. javascript [showdown.js](https://github.com/coreyti/showdown) 源码中提供了 example 可参考修改。

三. [PHP Markdown 模块](http://blog.aboutc.net/php/9/php-markdown-extension)，使用方法：

	$markdown_str = <<<EOD
	A First Level Header
	====================
	
	A Second Level Header
	---------------------
	
	Now is the time for all good men to come to
	the aid of their country. This is just a
	regular paragraph.
	
	The quick brown fox jumped over the lazy
	dog's back.
	
	### Header 3
	
	> This is a blockquote.
	>
	> This is the second paragraph in the blockquote.
	>
	> ## This is an H2 in a blockquote
	EOD;
	
	$md = MarkdownDocument::createFromString($markdown_str);
	$md->compile();
	echo $md->getHtml();

另，源码中提供了 examples 可参考，本例摘自 examples/simple_usage.php。 如需其他语言，请自行Google。

另外 [PhpStorm](http://www.jetbrains.com/phpstorm/) 是一款优秀的编辑器，推荐使用，查看 [PhpStorm 安装 Markdown 插件](http://blog.aboutc.net/php/41/phpstorm-install-markdown-plugin)，提供了 Markdown 预览功能。

参考

	markdown官网: http://daringfireball.net/projects/markdown/
	net tuts+: http://net.tutsplus.com/tutorials/other/vim-essential-plugin-markdown-to-html/
