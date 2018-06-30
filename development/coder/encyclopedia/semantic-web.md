Web语义化
=========

在昨天和做SEO的同学聊了一会儿，当然我没有学会搜索引擎优化的技巧和知识，但在此之前一直对HTML5中header、footer、sidebar、article等标签嗤之以鼻，觉得这个和div没有什么区别，顶多添加个style的事儿，当然自己在写页面的时候也从来没用过h1~h6这几个标签，相信还有很多和我有同样疑惑的前端小菜，特此解一下惑。

### 一个小问题

先看一段HTML代码

    <table>
        <tr>
            <td colspan="2">Student List</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>Age</td>
        </tr>
        <tr>
            <td>Byron</td>
            <td>24</td>
        </tr>
        <tr>
            <td>Vincent</td>
            <td>25</td>
        </tr>
        <tr>
            <td>Casper</td>
            <td>27</td>
        </tr>
    </table>

就算是没有看到页面效果，相信很多同学看到内容后也能明白这个表格在描述什么，但是计算机（搜索引擎）却不明白，计算机并不知道Name或者Age代表title，而下面的是内容。但如果这么写

    <table>
        <caption>Student List</caption>
        <thead>
            <tr>
                <th>Name</th>
                <th>Age</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Byron</td>
                <td>24</td>
            </tr>
            <tr>
                <td>Vincent</td>
                <td>25</td>
            </tr>
            <tr>
                <td>Casper</td>
                <td>27</td>
            </tr>
        </tbody>
    </table>

这样即使我们不看内容也能了解这大概是什么，计算机也会理解哪些是标题，哪些是header，哪些是内容。

### Web 语义化

这个东西我也是和同学们聊了聊天儿，自己翻了些资料，所以这里仅说一下个人的理解。HTML的每个标签都有其特定含义（语义），Web语义化是指使用语义恰当的标签，使页面有良好的结构，页面元素有含义，能够让人和搜索引擎都容易理解。

如果可以在合适的位置使用恰当的标签，那么写出来的页面语义明确，结构清晰，搜索引擎也可以认出哪些是页面重要内容，予以较高的权值。h1~h6这几个标签在搜索引擎中权值非常高，用它们作页面标题就是一个简单的SEO优化了，知道了这个你还使用DIV+CSS来写标题吗？（不是说直接用h1~h6标签做标题，要这几个标签+CSS）

回到开始时的疑惑，这时候header、footer、sidebar、article等标签的出现就不是那么让人困惑了，HTML5的一大革新就是语义化标签的完善

![](http://biang.io/biangpic/blog/ef5872394e6f875fbf28c59754fdb3f5.jpg)

使用这样结构写出的网页其语义显而易见。在有些面试的时候会问到类似strong 和font-weight: bold有什么区别，这时候就可以从语义化的角度解答了。

### 一些标签语义

    标签	语义
    h1~h6	标题
    th	table的header
    p	段落
    ul	无序列表
    ol	排序列表
    dl	definition list，定义列表
    dt	definition title，定义名称
    dd	definition  description定义描述
    em	emphasized，局部强调,段落内强调
    strong	更强烈的强调，全文强调

### 一个例子

我经常访问网易博客，看看它用了哪些语义化标签

![](http://biang.io/biangpic/blog/96c49be842befdac404b766f4cb84ca8.jpg)

![](http://biang.io/biangpic/blog/298a99f18b24582a60b65b5874faa14c.jpg)

![](http://biang.io/biangpic/blog/65cf350849f38ae735d7d438a7087f06.jpg)

即使可以轻松通过div+css实现的效果，网易也选择了h1、h2、strong等标签
