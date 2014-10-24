SlidesJS插件的使用
==================

### 基本的HTML

	<pre>
	<div id="slides">
		<img src="test1.jpg" />
		<img src="test1.jpg" />
		<img src="test1.jpg" />
		<img src="test1.jpg" />
		<img src="test1.jpg" />
	</div>
	</pre>
   
### 一、设置宽度和高度幻灯片。
 
    $(function(){
      $("#slides").slidesjs({
        width: 940,
        height: 528
      });
    });
 
### 二、设置在幻灯片中的第一张幻灯片。

    $(function(){
      $("#slides").slidesjs({
        start: 3
      });
    });

### 三、下一个和以前的按钮设置。

    $(function(){
      $("#slides").slidesjs({
        navigation: {
          active: true,
            // [boolean] 生成下一个和以前的按钮。
            // 可以设置为false，并使用自己的按钮。
            // 用户定义的按钮，必须具备以下条件：
            // 上一个按钮：class="slidesjs-previous slidesjs-navigation"
            // 下一个按钮：class="slidesjs-next slidesjs-navigation"
          effect: "slide"
            //可以是 "slide" 或者 "fade".
       }
      });
    });

### 四、分页设置。

    $(function(){
      $("#slides").slidesjs({
        pagination: {
          active: true,
            // [boolean] 创建分页项
            // 不能使用自己的分页
          effect: "slide"
            // [string] 可以是 "slide" 或者 "fade".
        }
      });
    });

### 五、播放和停止按钮设置。

	$(function(){
	  $("#slides").slidesjs({
		play: {
		  active: true,
			// [boolean] 生成的播放和停止按钮.
			//不能使用自己的按键。
		  effect: "slide",
			// [string] 可以是 "slide" 或者 "fade".
		  interval: 5000,
			// [number] 每张幻灯片上花费的时间以毫秒为单位。
		  auto: false,
			// [boolean] 加载开始播放幻灯片。
		  swap: true,
			// [boolean] 显示/隐藏停止和播放按钮
		  pauseOnHover: false,
			// [boolean] 鼠标经过暂停正在播放的幻灯片。
		  restartDelay: 2500
			// [number] 重新启动延迟无效幻灯片。
		}
	  });
	});

### 六、效果设置。

	$(function(){
	  $("#slides").slidesjs({
		effect: {
		  slide: {
			// 滑动效果设置.
			speed: 200
			  // [number] 速度以毫秒为单位的幻灯片动画。
		  },
		  fade: {
			speed: 300,
			  // [number] 速度以毫秒为单位的幻灯片动画。
			crossfade: true
			  // [boolean] 交叉过度效果.
		  }
		}
	  });
	});

### 七、回调函数的使用。

	$(function(){
	  $("#slides").slidesjs({
		callback: {
		  loaded: function(number) {
			//第一次加载幻灯片时
		  },
		  start: function(number) {
			//幻灯片开始切换时
		  },
		  complete: function(number) {
			//幻灯片切换完成时
		  }
		}
	  });
	});

