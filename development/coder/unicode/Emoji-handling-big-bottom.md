# emoji处理方式大起底

### emoji资料

今天研究了emoji，挺有意思，资料挺多，摘要一些信息给大家分享，也算是自己记录学习。

### emoji介绍

Emoji （絵文字，词义来自日语えもじ，e-moji，moji在日语中的含义是字符）是一套起源于日本的12x12像素表情符号，由栗田穣崇(Shigetaka Kurit)创作，最早在日本网络及手机用户中流行，自苹果公司发布的iOS 5输入法中加入了emoji后，这种表情符号开始席卷全球，目前emoji已被大多数现代计算机系统所兼容的Unicode编码采纳，普遍应用于各种手机短信和社交网络中。近期，更是有不少网友用emoji图案玩猜字游戏，享受这种表情文化带来的乐趣。

关于emoji的发音：很多人第一眼见到emoji便会下意识将其误读作“一磨叽”，其实不然，emoji音译过来大概读作“诶磨叽”，当中“e”的发音颇似字母abc的a的发音。

最初日本的三大电信运营商各自有不同的字符定义，分别是DoCoMo、KDDI和Softbank。随着iOS内置了Softbank的版本，emoji在全球范围内风靡（iOS5版本以前）。而Google又自己定义了一套emoji字符。iOS5以后，apple采用了unicode定义的emoji字符（iOS5版本以后）。

unicode定义的emoji是四个字符，softbank为3个字符，emoji的四个字符从存储到展示对应没有做过考虑的系统来说，简直就是灾难。

### 面临问题：

插入Emoji表情，保存到数据库时报错：

    SQLException: Incorrect string value: '\xF0\x9F\x98\x84' for column 'review' at row 1

UTF-8编码有可能是两个、三个、四个字节。Emoji表情是4个字节，而MySQL的utf8编码最多3个字节，所以数据插不进去。

### 解决方案：过滤解决

把emoji直接过滤掉，简单方便有效。虽然损失了几个emoji字符，但强过不至于导致整条记录丢失。

    public static String removeNonBmpUnicode(String str) {  
       if (str == null) {  
           return null;  
       }  
       str = str.replaceAll("[^\\u0000-\\uFFFF]", "");  
      return str;  
    }  

这种方案能预防能解决问题，并且还能是程序更加健壮，但是从用户体验上来说并不好，用户发的emoji表情丢了，看下面的解决方案。

### 解决方案：将Mysql的编码从utf8转换成utf8mb4。

从 MySQL 5.5.3 开始，MySQL 支持一种 utf8mb4 的字符集，这个字符集能够支持 4 字节的 UTF8 编码的字符。 utf8mb4 字符集能够完美地向下兼容 utf8 字符串。在数据存储方面，当一个普通中文字符存入数据库时仍然占用 3 个字节，在存入一个 Unified Emoji 表情的时候，它会自动占用 4 个字节。所以在输入输出时都不会存在乱码的问题了。

要使用 MySQL 的这个特性，首先需要把 MySQL 升级到 5.5.3 以上的版本。

其次，需要修改数据结构中的字符集为 utf8mb4 ，如 `utf8mb4_general_ci` 。由于 utf8mb4 是 utf8 的超集，从 utf8 升级到 utf8mb4 不会有任何问题，直接升级即可；如果从别的字符集如 gb2312 或者 gbk 转化而来，一定要先备份数据库。

然后，修改 MySQL 的配置文件 /etc/my.cnf，修改连接默认字符集为 utf8mb4 ，如果是自己写的 PHP 脚本，也可以在连接数据库以后首先执行一句 SQL: SET NAMES utf8mb4;。这时候，PHP 应该就可以正常保存 Emoji 到数据库了。

这种方式可能带来的问题：

存储：在数据表中，对于变长的字段（如VARCHAR2，TEXT），utf8mb4最大可存储的字符可能少于utf8系列的collation；在索引中，对于文本类型的字段，utf8mb4可索引的字符少于utf8系列的collations。如InnoDB的索引最多使用767字节。如果使用utf8mb4，每一个字符都会预留4字节做索引，而utf8则预留3字节。故此前者是191个字符，后者是255个字符。。

性能：由于以上原因，加上字符集大，utf8mb4的性能可能比utf8系列的collations低，可以参考stackoverfolow上的一个测试结果：http://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci，差异不是特别大。

运维：如果一个大的环境内，如果其他的数据库都是utf8模式，把其中某个库设置为utf8mb4模式，在后续交接运维可能会造成问题，遗留下坑。

上下游：数据库支持unicode的emoji存储，上下游不一定支持。比如mysql客户端驱动（低版本的jdbc就不行）可能不支持utf8mb4，或者DDL的中间件不支持utf8mb4。web端处理utf8mb4字符展示，这些都有可能影响emoji的存储活着展示。

从上面的信息，从数据库层面如果不是特别看重存储，性能，运维并能解决上下游的问题，数据库是完全可以支持emoji的，但是有个新问题没有解决，emoji在iOS上展示OK，andriod设备如何展示emoji表情？

### 解决方案：转义解决

1：unicode emoji转softbank的emoji。

我们知道unicode emoji是4个字节，softbank定义的emoji占用3个字节存储，通过emoji for php http://code.iamcal.com/php/emoji/，我们可以把unicode的emoji方式转换为softbank方式，从而实现不修改数据库，就能存储emoji，相对于数据库层面的解决问题的方式，动作要小的多，并且也不会有性能，运维等方面的问题。但是有个不可避免的问题是，Softbank方式已经不再维护，所以新增加的emoji表情，Softbank中都没有，会造成部分emoji表情丢失的情况。

2：ubb

UBB代码是HTML（标准通用标记语言下的一个应用）的一个变种，是Ultimate Bulletin Board （国外的一个BBS程序）采用的一种特殊的TAG。您也许已经对它很熟悉了。UBB代码很简单，功能很少，但是由于其Tag语法检查实现非常容易，所以不少网站引入了这种代码，以方便网友使用显示图片/链接/加粗字体等常见功能。

比如emoji的太阳符号，他的unicode emoji编码为U+2600，在存入数据库时，可以把它转换成  UBB 代码 [emoji]2600[/emoji] 保存，读取的时候，可以转换回来。当然针对不同的设备，比如andriod我们可以转义成andriod可以处理的emoji符号。

这种转移，可以很好解决iOS和Andriod显示emoji的问题，但是还存在几个问题。

1：andriod和iOS的emoji并不相同，相同的编码 可能在iOS上是太阳，而在andriod上是阴天，解决这种问题方式最好做下iOS和andriod下的emoji映射，同时可以在web上通过js转义处理。

2：性能，采用转义的方式处理，性能肯定会有所下降，但是可以容忍。

与UBB对应的是html转义，这种方式，其实和ubb有些类似， 使用 HTML转义字符 &#x2600;，结果和性能和UBB差不多，从规范化上来说，ubb方式更好一些。

### 参考资料

PHP-emoji转换表：http://code.iamcal.com/php/emoji/

unicode Emoji Symbols： http://www.unicode.org/~scherer/emoji4unicode/20091221/utc.html

emoji图标和unicode对应关系：http://www.easyapns.com/iphone-emoji-alerts

谈谈Unicode编码，简要解释UCS、UTF、BMP、BOM等名词：http://www.fmddlmyy.cn/text6.html

emoji在线转换工具：http://unicodey.com/js-emoji/demo.htm

Emoji表情图标在iOS与PHP之间通信及MySQL存储：http://blog.csdn.net/wildfireli/article/details/9370161

Mysql中校对集utf8_unicode_ci与utf8_general_ci的区别：http://hi.baidu.com/phpkoo/item/38238bd8505899e955347fca，http://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci

Mysql排序规则utf8_unicode_ci与utf8_general_ci的区别：http://justdo2008.iteye.com/blog/2162842

Unicode Character Sets：http://dev.mysql.com/doc/refman/5.0/en/charset-unicode-sets.html

MySQL设置utf8mb4编码：http://www.linuxidc.com/Linux/2014-07/104231.htm|

andriod支持emoji解决方案：http://blog.csdn.net/waylife/article/details/11095113

Supporting New Emojis on iOS 6：http://blog.manbolo.com/2012/10/29/supporting-new-emojis-on-ios-6

让MySql支持Emoji表情（MySQL中4字节utf8字符保存方法）：http://www.w2bc.com/Article/8533

如何处理emoji等4字节的Unicode字符：http://zhidao.baidu.com/link?url=z6PW1ya6plRBgFN7M2zdVLXUnmxYcH2_VYK8nW9Yi9-kh2estgmJomw1LssmsA853WYHsRtulkJn2okq0a3TAUDQHIiMe7b0VS-FeGMNYUu

suppoting new emoji for ios6:http://blog.manbolo.com/2012/10/29/supporting-new-emojis-on-ios-6 

UTF-8格式emoji：http://punchdrunker.github.io/iOSEmoji/table_html/ios6/index.html 
