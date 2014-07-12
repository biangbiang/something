nodejs文件操作
============

### 读取文件

    var fs = require('fs');
    fs.readFile(filename, [options], callback)

options取值：

* encoding：文件内容编码，如果没有设定编码，获取到的是raw buffer
* flag：应该是文件操作模式，默认是"r"

example:

    fs.readFile('README.md', {encoding: 'utf8'}, function (err, data) {
        if (err) throw err;
        console.log(data);
    });
