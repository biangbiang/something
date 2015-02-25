html5学习笔记
=============

HTML5 是下一代的 HTML。

---

### 什么是 HTML5？

HTML5 将成为 HTML、XHTML 以及 HTML DOM 的新标准。

HTML 的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。

HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持。

### HTML5 是如何起步的？

HTML5 是 W3C 与 WHATWG 合作的结果。

编者注：W3C 指 World Wide Web Consortium，万维网联盟。

编者注：WHATWG 指 Web Hypertext Application Technology Working Group。

WHATWG 致力于 web 表单和应用程序，而 W3C 专注于 XHTML 2.0。在 2006 年，双方决定进行合作，来创建一个新版本的 HTML。

为 HTML5 建立的一些规则：

* 新特性应该基于 HTML、CSS、DOM 以及 JavaScript。
* 减少对外部插件的需求（比如 Flash）
* 更优秀的错误处理
* 更多取代脚本的标记
* HTML5 应该独立于设备
* 开发进程应对公众透明

### 新特性

HTML5 中的一些有趣的新特性：

* 用于绘画的 canvas 元素
* 用于媒介回放的 video 和 audio 元素
* 对本地离线存储的更好的支持
* 新的特殊内容元素，比如 article、footer、header、nav、section
* 新的表单控件，比如 calendar、date、time、email、url、search

### 浏览器支持

最新版本的 Safari、Chrome、Firefox 以及 Opera 支持某些 HTML5 特性。Internet Explorer 9 将支持某些 HTML5 特性。

---

## HTML 5 视频

许多时髦的网站都提供视频。HTML5 提供了展示视频的标准。

### Web 上的视频

直到现在，仍然不存在一项旨在网页上显示视频的标准。

今天，大多数视频是通过插件（比如 Flash）来显示的。然而，并非所有浏览器都拥有同样的插件。

HTML5 规定了一种通过 video 元素来包含视频的标准方法。

### 视频格式

当前，video 元素支持三种视频格式：

    格式	IE	Firefox	Opera	Chrome	Safari
    Ogg	No	3.5+	10.5+	5.0+	No
    MPEG 4	9.0+	No	No	5.0+	3.0+
    WebM	No	4.0+	10.6+	6.0+	No

* Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件
* MPEG4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
* WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件

### 如何工作

如需在 HTML5 中显示视频，您所有需要的是：

    <video src="movie.ogg" controls="controls">
    </video>

control 属性供添加播放、暂停和音量控件。

包含宽度和高度属性也是不错的主意。

<video> 与 </video> 之间插入的内容是供不支持 video 元素的浏览器显示的：

    <video src="movie.ogg" width="320" height="240" controls="controls">
    Your browser does not support the video tag.
    </video>

上面的例子使用一个 Ogg 文件，适用于Firefox、Opera 以及 Chrome 浏览器。

要确保适用于 Safari 浏览器，视频文件必须是 MPEG4 类型。

video 元素允许多个 source 元素。source 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：

    <video width="320" height="240" controls="controls">
      <source src="movie.ogg" type="video/ogg">
      <source src="movie.mp4" type="video/mp4">
    Your browser does not support the video tag.
    </video>

### Internet Explorer

Internet Explorer 8 不支持 video 元素。在 IE 9 中，将提供对使用 MPEG4 的 video 元素的支持。

### <video> 标签的属性

    属性	值	描述
    autoplay	autoplay	如果出现该属性，则视频在就绪后马上播放。
    controls	controls	如果出现该属性，则向用户显示控件，比如播放按钮。
    height	pixels	设置视频播放器的高度。
    loop	loop	如果出现该属性，则当媒介文件完成播放后再次开始播放。
    preload	preload	
    如果出现该属性，则视频在页面加载时进行加载，并预备播放。
    如果使用 "autoplay"，则忽略该属性。
    src	url	要播放的视频的 URL。
    width	pixels	设置视频播放器的宽度。
 
---

