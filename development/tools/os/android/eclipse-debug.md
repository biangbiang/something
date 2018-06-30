Android 软件开发之如何使用Eclipse Debug调试程序详解
===================================================

## 1.在程序中添加一个断点

如果所示：在Eclipse中添加了一个程序断点 

![](http://biang.io/biangpic/blog/ae172d5be3d715f082578535c2cfe875.jpg)

在Eclipse中一共有三种添加断点的方法

第一种： 在红框区域右键出现菜单后点击第一项 Toggle Breakpoint 将会在你右键代码的哪一行添加一个程序断点 (同样的操作方可取消程序断点)

第二种： 在红框区域双击鼠标左键将会在你双击代码的哪一行添加一个程序断点 (同样的操作方可取消程序断点) 

![](http://biang.io/biangpic/blog/eee31c2fe970296897a4c3e66cff9102.jpg)

第三种 ：在光标停留的地方使用快捷键 Ctrl + Shift + B 将会在光标停留的这一行添加一个程序断点 (同样的操作方可取消程序断点)
 
![](http://biang.io/biangpic/blog/b6178e34456b2f888226f08a3ac272df.jpg)

## 2.运行Debug 调试 让程序停留在添加的断点上 

如下图所示，在红框内点击下拉菜单选中需要调试的项目 则开始运行Debug调试 

如果不在下拉表中选直接点击表示Debug运行默认项目(默认项目为上一次运行的项目)

Debug调试 快捷键为单击F11 

![](http://biang.io/biangpic/blog/b061c7ecb21eccd9a0f4ff01096543ad.jpg)

分析一下如何科学的添加程序断点， 上图中我为了加断点查看生成出来随机数的值我一共添加了6个程序断点，绿框表示最为科学的断点位置 ，红框表示不科学的位置。 我们分析一下为什么， 如果switch case 中的代码片段过长 或者 case 的数量过多 如果采用红框的方式来添加程序断点，程序员须要添加很多程序断点万一有疏漏 所以会很难快速定位代码执行到了那里 ，如果使用绿框的方式添加程序断点，程序员只须要在断点出按 F6 单步跳过这一行代码就会走进正确的case中方便继续调试。 

![](http://biang.io/biangpic/blog/2f5a514670dfba03cc858c2d21de0277.jpg)

Debug调试运行后，程序停在了红框处，按F6单步跳过 发现随机数为4 程序停留在了绿框中，程序员可以迅速定位random的值为4

## 3.程序停留后查看变量的数值

蓝框中的内容表示为断点的入口方法， 就好比你的断点是从那个方法进来的，学会看这个真的非常重要， 好比我现在明确知道我的一个方法在被调用的时候方法中会出现错误，但是这个方法在程序中100个地方都在调用，我可能断定实在那里调用的时候出的错误，我不可能在100个调用它的地方都加一个断点，我可以在方法中添加程序断点 然后在篮框中查看程序是从那个地方走进这个方法的，便可以快速定位问题所在。

绿框中可以查看当前方法中所有变量的值，但是如果变量非常多在这里看就比较麻烦，可以使用红框的方法查看。

红框中可以右键变量名点击咖啡框中的watch 后 在紫框中Expressions 就可以看到变量的数值了。

BreakPoints 中会记录程序中添加过多少程序断点。 

![](http://biang.io/biangpic/blog/4ad96b688a886a49c14ffc23bf348464.jpg)

## 4.分享一些Eclipse中Debug的一些小技巧

watch 过的变量 和我们自己加的程序断点不会被Eclipse 自动删除 除非我们手动删除否则会一直留在紫框中，这些数值会拖慢Eclipse 开发工具，如果过多的话很可能会造成 Eclipse 崩溃(有可能是Eclipse的BUG)，让开发变得非常痛苦，所以雨松MOMO在这里建议大家在每次Debug调试的时候将紫框中之前 加的程序断点 和 watch过的变量 全不手动清空，只添加这一次调试须要的断点就可以了，这样的话 Eclipse 就不会被这些拖慢进程的东西所导致崩溃。

## 5.连接真机调试

第一步 打开自己的手机在设置中选择应用程序 然后选择开发 然后选中USB调试。

第二步 用USB线连接手机到电脑，一般情况会自动安装驱动，如果无法安装驱动的话 就去下载一个豌豆荚 或者91助手，让它帮我们手机自动安装驱动 很方便的。

第三步 驱动安装成功后会在Device中看到真机(红框中) 绿框中为android电脑模拟器

![](http://biang.io/biangpic/blog/dc20ba1e9569a3fd99d78384acaafafb.jpg)

运行项目后弹出设备选择窗口 第一个为模拟器 第二个红框内的为我连接电脑的真机 MOTO的里程碑，选择完后点击OK 就可以通过真机来调试程序了，简单吧？是不是很给力呢呵呵。

![](http://biang.io/biangpic/blog/6afec3b8944972daeeb9e6d016acd9f5.jpg)

## 6.Android 开发中Log信息的打印

本人做过J2ME 开发 Android开发 iPhone开发 发现J2ME 的模拟器 还有Iphone的模拟器都非常给力速度很很快(模拟器比真机快) 唯独android的模拟器 是最不给力的 (真机比模拟器快) 实在是慢的不行 连接上真机可以快一点 但是一样还是慢 尤其是Debug的时候 简直是太不给力了（发点牢骚大家别介意哦 > - <）所以有时候我在开发Android的时候不到万不得已我不去Debug 我会使用Log去打印我须要的数据 下面我教大家如何在Andoid下打印Log信息。希望大家都学会使用log.
 
    public class testActivity extends Activity {  
         
        /**  
        * 返回一个随机数  
        * @param botton  
        * @param top  
        * @return  
        */  
        private int UtilRandom(int botton, int top) {  
        return ((Math.abs(new Random().nextInt()) % (top - botton)) + botton);  
        }  
        @Override  
        public void onCreate(Bundle savedInstanceState) {  
            super.onCreate(savedInstanceState);  
           
            
            int a = UtilRandom(0,5);  
            int b = UtilRandom(0,5);  
            int c = UtilRandom(0,5);  
            int d = UtilRandom(0,5);  
            int e = UtilRandom(0,5);  
        
              
              
              
            Log.v("random", a +"");  
            Log.v("random", b +"");  
            Log.v("random", c +"");  
            Log.v("random", d +"");  
            Log.v("random", e +"");  
              
            setContentView(R.layout.main);  
        }  
    } 

常用的Log有5个：Log.v()Log.d()Log.i() Log.w() Log.e()。

根据首字母对应VERBOSE，DEBUG,INFO, WARN，ERROR。

以上这些Log系统都会打印出来。

打开LogCat页面发现系统打印了很多Log信息 我们不好定位出刚才自己打的Log，如图所示点击红框内的“+”符号弹出下方窗口后在Filter Name : 和 by Log Tag: (蓝框内)填写我们刚才打的LOG tag "random"注意这两项都必需填写 然后单击OK后 方可在绿框中看到我们刚才打的random的Log 。怎么样还是很简单的吧 呵呵。 

![](http://biang.io/biangpic/blog/756278b0f1d3066e87c04419cb371999.jpg)

## 最后附上Eclipse 在开发中使用到的快捷键很实用噢(转载)
 
    Ctrl+1 快速修复(最经典的快捷键,就不用多说了)
    Ctrl+D: 删除当前行 
    Ctrl+Alt+↓ 复制当前行到下一行(复制增加)
    Ctrl+Alt+↑ 复制当前行到上一行(复制增加)
    Alt+↓ 当前行和下面一行交互位置(特别实用,可以省去先剪切,再粘贴了)
    Alt+↑ 当前行和上面一行交互位置(同上)
    Alt+← 前一个编辑的页面
    Alt+→ 下一个编辑的页面(当然是针对上面那条来说了)
    Alt+Enter 显示当前选择资源(工程,or 文件 or文件)的属性
    Shift+Enter 在当前行的下一行插入空行(这时鼠标可以在当前行的任一位置,不一定是最后)
    Shift+Ctrl+Enter 在当前行插入空行(原理同上条)
    Ctrl+Q 定位到最后编辑的地方
    Ctrl+L 定位在某行 (对于程序超过100的人就有福音了)
    Ctrl+M 最大化当前的Edit或View (再按则反之)
    Ctrl+/ 注释当前行,再按则取消注释
    Ctrl+O 快速显示 OutLine
    Ctrl+T 快速显示当前类的继承结构
    Ctrl+W 关闭当前Editer
    Ctrl+K 参照选中的Word快速定位到下一个
    Ctrl+E 快速显示当前Editer的下拉列表(如果当前页面没有显示的用黑体表示)
    Ctrl+/(小键盘) 折叠当前类中的所有代码
    Ctrl+×(小键盘) 展开当前类中的所有代码
    Ctrl+Space 代码助手完成一些代码的插入(但一般和输入法有冲突,可以修改输入法的热键,也可以暂用Alt+/来代替)
    Ctrl+Shift+E 显示管理当前打开的所有的View的管理器(可以选择关闭,激活等操作)
    Ctrl+J 正向增量查找(按下Ctrl+J后,你所输入的每个字母编辑器都提供快速匹配定位到某个单词,如果没有,则在stutes line中显示没有找到了,查一个单词时,特别实用,这个功能Idea两年前就有了)
    Ctrl+Shift+J 反向增量查找(和上条相同,只不过是从后往前查)
    Ctrl+Shift+F4 关闭所有打开的Editer
    Ctrl+Shift+X 把当前选中的文本全部变味小写
    Ctrl+Shift+Y 把当前选中的文本全部变为小写
    Ctrl+Shift+F 格式化当前代码
    Ctrl+Shift+P 定位到对于的匹配符(譬如{}) (从前面定位后面时,光标要在匹配符里面,后面到前面,则反之)

下面的快捷键是重构里面常用的,本人就自己喜欢且常用的整理一下(注:一般重构的快捷键都是Alt+Shift开头的了)

    Alt+Shift+R 重命名 (是我自己最爱用的一个了,尤其是变量和类的Rename,比手工方法能节省很多劳动力)
    Alt+Shift+M 抽取方法 (这是重构里面最常用的方法之一了,尤其是对一大堆泥团代码有用)
    Alt+Shift+C 修改函数结构(比较实用,有N个函数调用了这个方法,修改一次搞定)
    Alt+Shift+L 抽取本地变量( 可以直接把一些魔法数字和字符串抽取成一个变量,尤其是多处调用的时候)
    Alt+Shift+F 把Class中的local变量变为field变量 (比较实用的功能)
    Alt+Shift+I 合并变量(可能这样说有点不妥Inline)
    Alt+Shift+V 移动函数和变量(不怎么常用)
    Alt+Shift+Z 重构的后悔药(Undo)

编辑

    作用域 功能 快捷键 
    全局 查找并替换 Ctrl+F 
    文本编辑器 查找上一个 Ctrl+Shift+K 
    文本编辑器 查找下一个 Ctrl+K 
    全局 撤销 Ctrl+Z 
    全局 复制 Ctrl+C 
    全局 恢复上一个选择 Alt+Shift+↓ 
    全局 剪切 Ctrl+X 
    全局 快速修正 Ctrl1+1 
    全局 内容辅助 Alt+/ 
    全局 全部选中 Ctrl+A 
    全局 删除 Delete 
    全局 上下文信息 Alt+？
    Alt+Shift+?
    Ctrl+Shift+Space 
    Java编辑器 显示工具提示描述 F2 
    Java编辑器 选择封装元素 Alt+Shift+↑ 
    Java编辑器 选择上一个元素 Alt+Shift+← 
    Java编辑器 选择下一个元素 Alt+Shift+→ 
    文本编辑器 增量查找 Ctrl+J 
    文本编辑器 增量逆向查找 Ctrl+Shift+J 
    全局 粘贴 Ctrl+V 
    全局 重做 Ctrl+Y 

查看

    作用域 功能 快捷键 
    全局 放大 Ctrl+= 
    全局 缩小 Ctrl+- 

窗口

    作用域 功能 快捷键 
    全局 激活编辑器 F12 
    全局 切换编辑器 Ctrl+Shift+W 
    全局 上一个编辑器 Ctrl+Shift+F6 
    全局 上一个视图 Ctrl+Shift+F7 
    全局 上一个透视图 Ctrl+Shift+F8 
    全局 下一个编辑器 Ctrl+F6 
    全局 下一个视图 Ctrl+F7 
    全局 下一个透视图 Ctrl+F8 
    文本编辑器 显示标尺上下文菜单 Ctrl+W 
    全局 显示视图菜单 Ctrl+F10 
    全局 显示系统菜单 Alt+- 

导航

    作用域 功能 快捷键 
    Java编辑器 打开结构 Ctrl+F3 
    全局 打开类型 Ctrl+Shift+T 
    全局 打开类型层次结构 F4 
    全局 打开声明 F3 
    全局 打开外部javadoc Shift+F2 
    全局 打开资源 Ctrl+Shift+R 
    全局 后退历史记录 Alt+← 
    全局 前进历史记录 Alt+→ 
    全局 上一个 Ctrl+, 
    全局 下一个 Ctrl+. 
    Java编辑器 显示大纲 Ctrl+O 
    全局 在层次结构中打开类型 Ctrl+Shift+H 
    全局 转至匹配的括号 Ctrl+Shift+P 
    全局 转至上一个编辑位置 Ctrl+Q 
    Java编辑器 转至上一个成员 Ctrl+Shift+↑ 
    Java编辑器 转至下一个成员 Ctrl+Shift+↓ 
    文本编辑器 转至行 Ctrl+L 

搜索

    作用域 功能 快捷键 
    全局 出现在文件中 Ctrl+Shift+U 
    全局 打开搜索对话框 Ctrl+H 
    全局 工作区中的声明 Ctrl+G 
    全局 工作区中的引用 Ctrl+Shift+G 

文本编辑

    作用域 功能 快捷键 
    文本编辑器 改写切换 Insert 
    文本编辑器 上滚行 Ctrl+↑ 
    文本编辑器 下滚行 Ctrl+↓ 

文件

    作用域 功能 快捷键 
    全局 保存 Ctrl+X 
    Ctrl+S 
    全局 打印 Ctrl+P 
    全局 关闭 Ctrl+F4 
    全局 全部保存 Ctrl+Shift+S 
    全局 全部关闭 Ctrl+Shift+F4 
    全局 属性 Alt+Enter 
    全局 新建 Ctrl+N 

项目

    作用域 功能 快捷键 
    全局 全部构建 Ctrl+B 

源代码

    作用域 功能 快捷键 
    Java编辑器 格式化 Ctrl+Shift+F 
    Java编辑器 取消注释 Ctrl+\ 
    Java编辑器 注释 Ctrl+/ 
    Java编辑器 添加导入 Ctrl+Shift+M 
    Java编辑器 组织导入 Ctrl+Shift+O 
    Java编辑器 使用try/catch块来包围 未设置，太常用了，所以在这里列出,建议自己设置。
    也可以使用Ctrl+1自动修正。 

运行

    作用域 功能 快捷键 
    全局 单步返回 F7 
    全局 单步跳过 F6 
    全局 单步跳入 F5 
    全局 单步跳入选择 Ctrl+F5 
    全局 调试上次启动 F11 
    全局 继续 F8 
    全局 使用过滤器单步执行 Shift+F5 
    全局 添加/去除断点 Ctrl+Shift+B 
    全局 显示 Ctrl+D 
    全局 运行上次启动 Ctrl+F11 
    全局 运行至行 Ctrl+R 
    全局 执行 Ctrl+U 

重构

    作用域 功能 快捷键 
    全局 撤销重构 Alt+Shift+Z 
    全局 抽取方法 Alt+Shift+M 
    全局 抽取局部变量 Alt+Shift+L 
    全局 内联 Alt+Shift+I 
    全局 移动 Alt+Shift+V 
    全局 重命名 Alt+Shift+R 
    全局 重做 Alt+Shift+Y

