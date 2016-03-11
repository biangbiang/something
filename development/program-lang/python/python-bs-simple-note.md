Python BeautifulSoup 简单笔记
=============================

Beautiful Soup 是用 Python 写的一个 HTML/XML 的解析器，它可以很好的处理不规范标记并生成剖析树。通常用来分析爬虫抓取的web文档。对于 不规则的 Html文档，也有很多的补全功能，节省了开发者的时间和精力。

Beautiful Soup 的官方文档齐全，将官方给出的例子实践一遍就能掌握。官方英文文档，中文文档

### 一 安装 Beautiful Soup

安装 BeautifulSoup 很简单，下载 BeautifulSoup  源码。解压运行

python setup.py install 即可。

测试安装是否成功。键入 import BeautifulSoup 如果没有异常，即成功安装

### 二 使用 BeautifulSoup

1. 导入BeautifulSoup ，创建BeautifulSoup 对象

        from BeautifulSoup import BeautifulSoup           # HTML
        from BeautifulSoup import BeautifulStoneSoup      # XML
        import BeautifulSoup                              # ALL
                                                                                                       
        doc = [
            '<html><head><title>Page title</title></head>',
            '<body><p id="firstpara" align="center">This is paragraph <b>one</b>.',
            '<p id="secondpara" align="blah">This is paragraph <b>two</b>.',
            '</html>'
        ]
        # BeautifulSoup 接受一个字符串参数
        soup = BeautifulSoup(''.join(doc))

2. BeautifulSoup对象简介

  用BeautifulSoup 解析 html文档时，BeautifulSoup将 html文档类似 dom文档树一样处理。BeautifulSoup文档树有三种基本对象。

  2.1. soup BeautifulSoup.BeautifulSoup

        type(soup)
        <class 'BeautifulSoup.BeautifulSoup'>

  2.2. 标记 BeautifulSoup.Tag

        type(soup.html)
        <class 'BeautifulSoup.Tag'>

  2.3 文本 BeautifulSoup.NavigableString

        type(soup.title.string)
        <class 'BeautifulSoup.NavigableString'>

3. BeautifulSoup 剖析树

  3.1 BeautifulSoup.Tag对象方法
  
  获取 标记对象（Tag）
  
  标记名获取法 ，直接用 soup对象加标记名，返回 tag对象.这种方式，选取唯一标签的时候比较有用。或者根据树的结构去选取，一层层的选择

        >>> html = soup.html
        >>> html
        <html><head><title>Page title</title></head><body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body></html>
        >>> type(html)
        <class 'BeautifulSoup.Tag'>
        >>> title = soup.title
        <title>Page title</title>

  content方法

  content方法 根据文档树进行搜索，返回标记对象（tag）的列表

        >>> soup.contents
        [<html><head><title>Page title</title></head><body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body></html>]
        >>> soup.contents[0].contents
        [<head><title>Page title</title></head>, <body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body>]
        >>> len(soup.contents[0].contents)
        2
        >>> type(soup.contents[0].contents[1])
        <class 'BeautifulSoup.Tag'>

  使用contents向后遍历树，使用parent向前遍历树
  
  next 方法
  
  获取树的子代元素，包括 Tag 对象 和 NavigableString 对象。。。

        >>> head.next
        <title>Page title</title>
        >>> head.next.next
        u'Page title'
        >>> p1 = soup.p
        >>> p1
        <p id="firstpara" align="center">This is paragraph<b>one</b>.</p>
        >>> p1.next
        u'This is paragraph'

  nextSibling 下一个兄弟对象 包括 Tag 对象 和 NavigableString 对象

        >>> head.nextSibling
        <body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body>
        >>> p1.next.nextSibling
        <b>one</b>

  与 nextSibling 相似的是 previousSibling，即上一个兄弟节点。
  
  replacewith方法
  
  将对象替换为，接受字符串参数

        >>> head = soup.head
        >>> head
        <head><title>Page title</title></head>
        >>> head.parent
        <html><head><title>Page title</title></head><body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body></html>
        >>> head.replaceWith('head was replace')
        >>> head
        <head><title>Page title</title></head>
        >>> head.parent
        >>> soup
        <html>head was replace<body><p id="firstpara" align="center">This is paragraph<b>one</b>.</p><p id="secondpara" align="blah">This is paragraph<b>two</b>.</p></body></html>
        >>>

  搜索方法
  
  搜索提供了两个方法，一个是 find，一个是findAll。这里的两个方法(findAll和 find)仅对Tag对象以及，顶层剖析对象有效，但 NavigableString不可用。
  
  findAll(name, attrs, recursive, text, limit, **kwargs)
  
  接受一个参数，标记名
  
  寻找文档所有 P标记，返回一个列表

        >>> soup.findAll('p')
        [<p id="firstpara" align="center">This is paragraph<b>one</b>.</p>, <p id="secondpara" align="blah">This is paragraph<b>two</b>.</p>]
        >>> type(soup.findAll('p'))
        <type 'list'>

  寻找 id="secondpara"的 p 标记，返回一个结果集

        >>> pid = type(soup.findAll('p',id='firstpara'))
        >>> pid
        <class 'BeautifulSoup.ResultSet'>

  传一个属性或多个属性对

        >>> p2 = soup.findAll('p',{'align':'blah'})
        >>> p2
        [<p id="secondpara" align="blah">This is paragraph<b>two</b>.</p>]
        >>> type(p2)
        <class 'BeautifulSoup.ResultSet'>

  利用正则表达式

        >>> soup.findAll(id=re.compile("para$"))
        [<p id="firstpara" align="center">This is paragraph<b>one</b>.</p>, <p id="secondpara" align="blah">This is paragraph<b>two</b>.</p>]

  读取和修改属性

        >>> p1 = soup.p
        >>> p1
        <p id="firstpara" align="center">This is paragraph<b>one</b>.</p>
        >>> p1['id']
        u'firstpara'
        >>> p1['id'] = 'changeid'
        >>> p1
        <p id="changeid" align="center">This is paragraph<b>one</b>.</p>
        >>> p1['class'] = 'new class'
        >>> p1
        <p id="changeid" align="center" class="new class">This is paragraph<b>one</b>.</p>
        >>>

  剖析树基本方法就这些，还有其他一些，以及如何配合正则表达式。具体请看官方文档

  3.2 BeautifulSoup.NavigableString对象方法
  
  NavigableString  对象方法比较简单，获取其内容

        >>> soup.title
        <title>Page title</title>
        >>> title = soup.title.next
        >>> title
        u'Page title'
        >>> type(title)
        <class 'BeautifulSoup.NavigableString'>
        >>> title.string
        u'Page title'

  至于如何遍历树，进而分析文档，已经 XML 文档的分析方法，可以参考官方文档。
