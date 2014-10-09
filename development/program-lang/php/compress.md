php压缩
=======

### ZipArchive 类

(PHP 5 >= 5.2.0, PECL zip >= 1.1.0)

### 简介

一个用 Zip 压缩的文件存档。

### 范例

This example opens a ZIP file archive test.zip and add the file /path/to/index.txt. as newname.txt.

Example #1 Open and add

	<?php
	$zip = new ZipArchive;
	if ($zip->open('test.zip') === TRUE) {
		$zip->addFile('/path/to/index.txt', 'newname.txt');
		$zip->close();
		echo 'ok';
	} else {
		echo 'failed';
	}
	?>
