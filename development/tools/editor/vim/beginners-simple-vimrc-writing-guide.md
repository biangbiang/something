# 初学者简易 .vimrc 编写指南

VIM 中可供用户定制的选项非常非常多，作为初学者，我们没有必要了解这么多东西。下面，滇狐简单列出了一些最常用的配置项，大家可以根据自己的需要将这些内容添加到自己的 .vimrc 中。

### 1 基本设置

    set nocp 

该命令指定让 VIM 工作在不兼容模式下。在 VIM 之前，出现过一个非常流行的编辑器叫 vi。VIM 许多操作与 vi 很相似，但也有许多操作与 vi 是不一样的。如果使用“:set cp”命令打开了兼容模式开关的话，VIM 将尽可能地模仿 vi 的操作模式。 

也许有许多人喜欢“最正统的 vi”的操作模式，对于初学者来说，vi 里许多操作是比较不方便的。 

举一个例子，VIM 里允许在 Insert 模式下使用方向键移动光标，而 vi 里在 Insert 模式下是不能移动光标的，必须使用 ESC 退回到 Normal 模式下才行。 

再举一个例子，vi 里使用 u 命令可以撤消一次先前的操作，再次按下 u 时，将撤消“撤消”这个动作本身，也就是我们常说的“重复”(redo)。而 VIM 里可以使用 u 命令撤消多步操作，“重复”使用的快捷键是 Ctrl + R。 

使用兼容模式后，VIM 将放弃这些新的功能，尽量模仿 vi 的各种操作方式。只有在不兼容模式下，才能更好地发挥 VIM 自身的特点。Bram 爷爷强烈推荐大家使用 VIM 的不兼容模式，滇狐也是这样推荐的。请务必在你的 .vimrc 中的第一行写上：“set nocp”。

    set ru 

该命令打开 VIM 的状态栏标尺。默认情况下，VIM 的状态栏标尺在屏幕底部，它能即时显示当前光标所在位置在文件中的行号、列号，以及对应的整个文件的百分比。打开标尺可以给文件的编辑工作带来一定方便。

    set hls 

搜索时高亮显示被找到的文本。该指令的功能在 vimtutor 中已经有过介绍，这里就不多说了。其实似乎许多人并不喜欢这个功能。

    set is 

搜索时在未完全输入完毕要检索的文本时就开始检索。vimtutor 对该命令也有过介绍，滇狐并不喜欢这个功能，因此滇狐自己的配置文件里是没有这条命令的。但是周围有朋友很喜欢这个，因此滇狐还是将它列在这里。

    syntax on 

打开关键字上色。进行程序设计的朋友应该都知道关键字上色是什么东西，因此这里就不多说了。不进行程序设计的朋友不妨也打开这个功能，虽然不一定能够用得着，但这个功能其实也是很好玩的。

    set backspace=indent,eol,start 

设想这样一个情况：当前光标前面有若干字母，我们按下 i 键进入了 Insert 模式，然后输入了 3 个字母，再按 5 下删除(Backspace)。默认情况下，VIM 仅能删除我们新输入的 3 个字母，然后喇叭“嘟嘟”响两声。如果我们“set backspace=start”，则可以在删除了新输入的 3 个字母之后，继续向前删除原有的两个字符。 

再设想一个情况：有若干行文字，我们把光标移到中间某一行的行首，按 i 键进入 Insert 模式，然后按一下 Backspace。默认情况下，喇叭会“嘟”一声，然后没有任何动静。如果我们“set backspace=eol”，则可以删除前一行行末的回车，也就是说将两行拼接起来。 

当我们设置了自动缩进后，如果前一行缩进了一定距离，按下回车后，下一行也会保持相同的缩进。默认情况下，我们不能在 Insert 模式下直接按 Backspace 删除行首的缩进。如果我们“set backspace=indent”，则可以开启这一项功能。 

上述三项功能，你可以根据自己的需要，选择其中一种或几种，用逗号分隔各个选项。建议把这三个选项都选上。

    set whichwrap=b,s,<,>,[,] 

默认情况下，在 VIM 中当光标移到一行最左边的时候，我们继续按左键，光标不能回到上一行的最右边。同样地，光标到了一行最右边的时候，我们不能通过继续按右跳到下一行的最左边。但是，通过设置 whichwrap 我们可以对一部分按键开启这项功能。如果想对某一个或几个按键开启到头后自动折向下一行的功能，可以把需要开启的键的代号写到 whichwrap 的参数列表中，各个键之间使用逗号分隔。以下是 whichwrap 支持的按键名称列表：

    b 

    在 Normal 或 Visual 模式下按删除(Backspace)键。

    s 

    在 Normal 或 Visual 模式下按空格键。

    h 

    在 Normal 或 Visual 模式下按 h 键。

    l 

    在 Normal 或 Visual 模式下按 l 键。

    < 

    在 Normal 或 Visual 模式下按左方向键。

    > 

    在 Normal 或 Visual 模式下按右方向键。

    ~ 

    在 Normal 模式下按 ~ 键(翻转当前字母大小写)。

    [ 

    在 Insert 或 Replace 模式下按左方向键。

    ] 

    在 Insert 或 Replace 模式下按右方向键。

-

    set encoding=utf-8 

设置当前字符编码为 UTF-8。UTF-8 是支持字符集最多的编码之一，在 UTF-8 下进行工作，会带来许多方便之处。由于 VIM 在运行过程中切换 encoding 会造成许多问题，如提示信息乱码、register 丢失等，因此强烈建议大家在启动 VIM 的时候把 encoding 设置为 UTF-8，在编辑非 UTF-8 的文件时，通过 fileencoding 来进行转码。

    set langmenu=zh_CN.UTF-8 

使用中文菜单，并使用 UTF-8 编码。如果没有这句的话，在非 UTF-8 的系统，如 Windows 下，用了 UTF-8 的 encoding 后菜单会乱码。

    language message zh_CN.UTF-8 

使用中文提示信息，并使用 UTF-8 编码。如果没有这句的话，在非 UTF-8 的系统，如 Windows 下，用了 UTF-8 的 encoding 后系统提示会乱码。

    set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1 

设置编码的自动识别。关于这条设置的详细含义，请参看这篇文章。

    set ambiwidth=double 

防止特殊符号无法正常显示。在 Unicode 中，许多来自不同语言的字符，如果字型足够近似的话，会把它们放在同一个编码中。但在不同编码中，字符的宽度是不一样的。例如中文汉语拼音中的 ā 就很宽，而欧洲语言中同样的字符就很窄。当 VIM 工作在 Unicode 状态时，遇到这些宽度不明的字符时，默认使用窄字符，这会导致中文的破折号“——”非常短，五角星“★”等符号只能显示一半。因此，我们需要设置 ambiwidth=double 来解决这个问题。

    filetype plugin indent on 

开启文件类型自动识别，启用文件类型插件，启用针对文件类型的自动缩进。

### 2 文本编辑设置

    set sw=4 

自动缩进的时候，缩进尺寸为 4 个空格。

    set ts=4 

Tab 宽度为 4 个字符。

    set et 

编辑时将所有 Tab 替换为空格。 

该选项只在编辑时将 Tab 替换为空格，如果打开一个已经存在的文件，并不会将已有的 Tab 替换为空格。如果希望进行这样的替换的话，可以使用这条命令“:retab”。

    set smarttab 

当使用 et 将 Tab 替换为空格之后，按下一个 Tab 键就能插入 4 个空格，但要想删除这 4 个空格，就得按 4 下 Backspace，很不方便。设置 smarttab 之后，就可以只按一下 Backspace 就删除 4 个空格了。

    set spell 

打开拼写检查。拼写有错的单词下方会有红色波浪线，将光标放在单词上，按 z= 就会出现拼写建议，按 ]s 可以直接跳到下一个拼写错误处。

### 3 断行设置

    set tw=78 

设置光标超过 78 列的时候折行。

    set lbr 

不在单词中间断行。设置了这个选项后，如果一行文字非常长，无法在一行内显示完的话，它会在单词与单词间的空白处断开，尽量不会把一个单词分成两截放在两个不同的行里。

    set fo+=mB 

打开断行模块对亚洲语言支持。m 表示允许在两个汉字之间断行，即使汉字之间没有出现空格。B 表示将两行合并为一行的时候，汉字与汉字之间不要补空格。该命令支持的更多的选项请参看用户手册。

### 4 C/C++ 编码设置

    set sm 

显示括号配对情况。打开这个选项后，当输入后括号(包括小括号、中括号、大括号) 的时候，光标会跳回前括号片刻，然后跳回来，以此显示括号的配对情况。

    set cin 

打开 C/C++ 风格的自动缩进。打开了自动缩进后，在编码的时候我们可以使用“V”命令选中一段文本后，按“=”将这段文本重新调整缩进格式，在一定程度上起到代码美化的作用。 

另外，打开了自动缩进后，在使用 TERM 下的 vim 的时，粘贴代码时常常会出现版式混乱的现象。那是因为 TERM 并不知道你现在正在粘贴代码，它直接“粘贴”动作向 vim 解释为键入，因此 vim 便按照设置的自动缩进格式为你的代码调整了缩进。不幸的是，粘贴进去的代码中本身已经包含了缩进，因此便出现了版式混乱的情况。在 gvim 中就不会出现这个问题，因为它能够知道你现在正在粘贴。 

知道了这个症状的来由，解决方案也就显而易见了：在粘贴的时候把所有自动缩进都关闭，粘贴完毕之后再打开。一个个手工关闭自动缩进未免过于繁琐，vim 为我们提供了一个很好用的命令，只要输入“:set paste”，就可以关闭所有自动缩进。粘贴完毕后再输入“:set nopaste”就可以重新打开原有的自动缩进设置了。

    set cino=:0g0t0(sus 

设定 C/C++ 风格自动缩进的选项，这里简要介绍一下这段代码里用到的选项的含义，cino 支持的选项还很多，更多选项请看用户手册。

    // :0
    //
    // switch 语句之下的 case 语句缩进 0 个空格，也就是说不缩进，与 switch 块平
    // 齐，使用这样风格：
    switch (x)
    {
    case 1:
        a = b;
        break;
    default:
    }

    // g0
    //
    // class、struct 等之下的访问权限控制语句，如 public、protected、private 等，
    // 相对 class、struct 等所在的块缩进 0 个空格，与 class 等块平齐，使用这样的
    // 风格：
    class foo
    {
    public:
        int a;
    private:
        int b;
    };

    // t0
    //
    // 如果函数返回值与函数名不在同一行，则返回值缩进 0 个空格，也就是说不缩进，如
    // 下所示：

    // set cino=t4
        int
    func1()
    {
    }

    // set cino=t0
    int
    func()
    {
    }

    // (sus
    //
    // 当一对括号跨越多行时，其后的行缩进前面 sw 指定的距离，效果如下：
    int a = (1 + 2 + 3
        + 4 + 5 + 6
        + 7 + 8) * 9;

-

    set ai 

打开普通文件类型的自动缩进。该自动缩进不如 cindent 智能，但它可以为你编辑非 C/C++ 文件提供一定帮助。

### 5 其它设置

    set selectmode= 

不使用 selectmode。

    set keymodel= 

不使用“Shift + 方向键”选择文本，“Shift + 方向键”代表向指定方向跳一个单词。如果你喜欢这项功能的话，可以使用“set keymodel=startsel,stopsel”打开它。

    set selection=inclusive 

指定在选择文本时，光标所在位置也属于被选中的范围。如果指定 selection=exclusive 的话，可能会出现某些文本无法被选中的情况。

    set wildmenu 

在命令模式下使用 Tab 自动补全的时候，将补全内容使用一个漂亮的单行菜单形式显示出来。

    colo torte 

选择 torte 配色方案。VIM 里内置了许多关键字上色的配色方案，另外你还可以到网上下载更多配色方案，或是自己编写。点击“编辑”→“调色板”，就能列出所有支持的配色方案。你可以把“调色板”子菜单剪下来，然后慢慢选择，挑出一个你最喜欢的配色方案来。

### 6 图形界面设置

    set nowrap 

指定不折行。如果一行太长，超过屏幕宽度，则向右边延伸到屏幕外面。如果使用图形界面的话，指定不折行视觉效果会好得多。

    set mousemodel=popup 

当右键单击窗口的时候，弹出快捷菜单。

    set guioptions+=b 

添加水平滚动条。如果你指定了不折行，那为窗口添加一个水平滚动条就非常有必要了。

    set guifont=Bitstream\ Vera\ Sans\ Mono\ 9 

设置图形界面下的字体。你可以点“编辑”→“选择字体”，然后在对话框中选出你喜欢的字体与字号，选择完毕后，先按几下 ESC 确认处在 Normal 模式下，然后输入这条命令：“:set guifont?”回车后 gvim 屏幕最下方会显示出你当前所用的字体的名称与字号。将获得的结果写到配置文件里面就可以了，需要注意一点，如果字体名称里面含有空格的话，在抄的时候需要在所有空格前面加一个斜杠。

### 7 条件选择

同一个配色方案，在 gvim 下和字符界面的 vim 下效果大相径庭，滇狐个人的习惯是，在 gvim 下使用 torte 配色方案，在 vim 下使用 ron 配色方案。因此我们有必要针对 gvim 和 vim 进行不同的设置。

另外，前面我们在 gvim 下不使用折行，开启水平滚动条，但在 vim 下，是没有滚动条可用的，因此还是有必要为 vim 保留自动折行。

条件选择设置的格式如下：

    if (has("gui_running"))
    " 图形界面下的设置
        set nowrap
        set guioptions+=b
        colo torte
    else
    " 字符界面下的设置
        set wrap
        colo ron
    endif

### 8 示例配置文件

下面给出一个滇狐推荐的初学者专用 (G)Vim 配置文件，里面没有太多个性化的设置，方便大家进一步扩展：

    set nocp

    " Tab related
    set ts=4
    set sw=4
    set smarttab
    set et
    set ambiwidth=double

    " Format related
    set tw=78
    set lbr
    set fo+=mB

    " Indent related
    set cin
    set ai
    set cino=:0g0t0(susj1

    " Editing related
    set backspace=indent,eol,start
    set whichwrap=b,s,<,>,[,]
    set mouse=a
    set selectmode=
    set mousemodel=popup
    set keymodel=
    set selection=inclusive

    " Misc
    set wildmenu
    set spell

    " Encoding related
    set encoding=utf-8
    set langmenu=zh_CN.UTF-8
    language message zh_CN.UTF-8
    set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1

    " File type related
    filetype plugin indent on

    " Display related
    set ru
    set sm
    set hls
    if (has("gui_running"))
        set guioptions+=b
        colo torte
        set nowrap
    else
        colo ron
        set wrap
    endif
    syntax on

    "=============================================================================
    " Platform dependent settings
    "=============================================================================

    if (has("win32"))

        "-------------------------------------------------------------------------
        " Win32
        "-------------------------------------------------------------------------

        if (has("gui_running"))
            set guifont=Bitstream_Vera_Sans_Mono:h9:cANSI
            set guifontwide=NSimSun:h9:cGB2312
        endif

    else

        if (has("gui_running"))
            set guifont=Bitstream\ Vera\ Sans\ Mono\ 9
        endif

    endif
