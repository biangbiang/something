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

### HTML input元素的value可以直接写在元素里面，而textarea元素的value却必须写在开始标签和结束标签之间，这是为什么？

textarea 用于支持多行文本，因此它的内容不适合以 DOM 的 value 属性来放置默认值。

不过，赋值的方式却是一样的，获取或设置 textarea 的 value 属性可以改变它的值。

### textarea到底是使用value还是innerhtml还是innerText来获取输入的内容的？

问：如题，就是看有的地方使用value，有的地方是innerHTML，还有的地方是用innerText，到底那种是对的，如果同时设置了多了，冲突了怎么办？

答1：应该用 value 属性。

答2：form 里的Dom元素(input select checkbox textarea radio)都是value
