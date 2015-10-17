firefox笔记
==========

### firebug显示NS\_ERROR\_NOT\_AVAILABLE错误

firfox连续弹出相同的值的框；如

    alert("1");
    alert("1");

会报一个“阻止此页面创建其他对话框的提示”，如果你勾选了， firebug里面会出现

    “Component returned failure code: 0x80040111 (NS_ERROR_NOT_AVAILABLE) [nsIDOMWindow.alert]”错误

切记，切记，否则调试会很郁闷

