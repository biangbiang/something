在mac中查看和转换txt文件编码
===========================

### 查看文件编码可以通过以下几种方式：

1. 在Vim中可以直接查看文件编码

        :set fileencoding
  
  即可显示文件编码格式。
  
  如果你只是想查看其它编码格式的文件或者想解决用Vim查看文件乱码的问题，那么你可以在
  
  `~/.vimrc` 文件中添加以下内容：
  
        set encoding=utf-8 fileencodings=ucs-bom,utf-8,cp936
  
  这样，就可以让vim自动识别文件编码（可以自动识别UTF-8或者GBK编码的文件），其实就是依照fileencodings提供的编码列表尝试，如果没有找到合适的编码，就用latin-1(ASCII)编码打开。
  
2. enca (如果你的系统中没有安装这个命令，可以用sudo yum install -y enca 安装 )查看文件编码
  
        $ enca filename
        filename: Universal transformation format 8 bits; UTF-8
        CRLF line terminators
  
  需要说明一点的是，enca对某些GBK编码的文件识别的不是很好，识别时会出现：
  
        Unrecognized encoding

---

### 文件编码转换

1. 在Vim中直接进行转换文件编码,比如将一个文件转换成utf-8格式

        :set fileencoding=utf-8

2. enconv 转换文件编码，比如要将一个GBK编码的文件转换成UTF-8编码，操作如下

        enconv -L zh_CN -x UTF-8 filename（这个命令对windows的ANSI编码非常有效）。

3. iconv 转换，iconv的命令格式如下：

        iconv -f encoding -t encoding inputfile

  比如将一个UTF-8 编码的文件转换成GBK编码

        iconv -f GBK -t UTF-8 file1 -o file2
