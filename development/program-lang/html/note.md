html学习笔记
==========

### :hover

:hover 选择器用于选择鼠标指针浮动在上面的元素。

:hover 选择器可用于所有元素，不只是链接。

在 CSS 定义中，:hover 必须位于 :link 和 :visited 之后（如果存在的话），这样样式才能生效。

* :hover 选择器可用于所有元素，不只是链接。
* :link 选择器设置指向未被访问页面的链接的样式，
* :visited 选择器用于设置指向已被访问的页面的链接，
* :active 选择器用于活动链接。

### 黑色背景 用什么颜色的字体

* #33CCCC
* #C5AAEE
* #B8E1E1
* #E4DAB4
* #B4D1E4
* #A3A3F5
* #333333 

### display:inline

display:inline is very limited and doesn't allow any block-level styling to added to it. You're better off using display:inline-block or using float:left. Keep in mind that if you use floats then you need to set the overflow of the parent element to overflow:auto (use visible for IE < 8) and this should work. Use inline-block first.
