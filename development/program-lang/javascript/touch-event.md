JS移动客户端--触屏滑动事件
==========================

移动端触屏滑动的效果其实就是图片轮播，在PC的页面上很好实现，绑定click和mouseover等事件来完成。但是在移动设备上，要实现这种轮播的效果，就需要用到核心的touch事件。处理touch事件能跟踪到屏幕滑动的每根手指。

以下是四种touch事件

    touchstart:     //手指放到屏幕上时触发

    touchmove:      //手指在屏幕上滑动式触发

    touchend:    //手指离开屏幕时触发

    touchcancel:     //系统取消touch事件的时候触发，这个好像比较少用

每个触摸事件被触发后，会生成一个event对象，event对象里额外包括以下三个触摸列表

    touches:     //当前屏幕上所有手指的列表

    targetTouches:      //当前dom元素上手指的列表，尽量使用这个代替touches

    changedTouches:     //涉及当前事件的手指的列表，尽量使用这个代替touches

这些列表里的每次触摸由touch对象组成，touch对象里包含着触摸信息，主要属性如下：

    clientX / clientY:      //触摸点相对浏览器窗口的位置

    pageX / pageY:       //触摸点相对于页面的位置

    screenX  /  screenY:    //触摸点相对于屏幕的位置

    identifier:        //touch对象的ID

    target:       //当前的DOM元素

注意：

手指在滑动整个屏幕时，会影响浏览器的行为，比如滚动和缩放。所以在调用touch事件时，要注意禁止缩放和滚动。

1.禁止缩放

通过meta元标签来设置。

    <meta name="viewport" content="target-densitydpi=320,width=640,user-scalable=no">

2.禁止滚动

preventDefault是阻止默认行为，touch事件的默认行为就是滚动。

    event.preventDefault();

案例：

下面给出一个案例，需在移动设备上才能看出效果。

1.定义touchstart的事件处理函数，并绑定事件：

    if(!!self.touch) self.slider.addEventListener('touchstart',self.events,false); 

    //定义touchstart的事件处理函数
    start:function(event){
    　　var touch = event.targetTouches[0]; //touches数组对象获得屏幕上所有的touch，取第一个touch
    　　startPos = {x:touch.pageX,y:touch.pageY,time:+new Date}; //取第一个touch的坐标值
    　　isScrolling = 0; //这个参数判断是垂直滚动还是水平滚动
    　　this.slider.addEventListener('touchmove',this,false);
    　　this.slider.addEventListener('touchend',this,false);
    },

触发touchstart事件后，会产生一个event对象，event对象里包括触摸列表，获得屏幕上的第一个touch,并记下其pageX,pageY的坐标。定义一个变量标记滚动的方向。此时绑定touchmove,touchend事件。

2.定义手指在屏幕上移动的事件，定义touchmove函数。

    //移动
    move:function(event){
    　　//当屏幕有多个touch或者页面被缩放过，就不执行move操作
    　　if(event.targetTouches.length > 1 || event.scale && event.scale !== 1) return;
    　　var touch = event.targetTouches[0];
    　　endPos = {x:touch.pageX - startPos.x,y:touch.pageY - startPos.y};
    　　isScrolling = Math.abs(endPos.x) < Math.abs(endPos.y) ? 1:0; //isScrolling为1时，表示纵向滑动，0为横向滑动
    　　if(isScrolling === 0){
    　　　　event.preventDefault(); //阻止触摸事件的默认行为，即阻止滚屏
    　　　　this.slider.className = 'cnt';
    　　　　this.slider.style.left = -this.index*600 + endPos.x + 'px';
    　　}
    },

同样首先阻止页面的滚屏行为，touchmove触发后，会生成一个event对象，在event对象中获取touches触屏列表，取得第一个touch,并记下pageX,pageY的坐标，算出差值，得出手指滑动的偏移量，使当前DOM元素滑动。

3.定义手指从屏幕上拿起的事件，定义touchend函数。

    //滑动释放
    end:function(event){
    　　var duration = +new Date - startPos.time; //滑动的持续时间
    　　if(isScrolling === 0){ //当为水平滚动时
    　　　　this.icon[this.index].className = '';
    　　　　if(Number(duration) > 10){ 
    　　　　　　//判断是左移还是右移，当偏移量大于10时执行
    　　　　　　if(endPos.x > 10){
    　　　　　　　　if(this.index !== 0) this.index -= 1;
    　　　　　　}else if(endPos.x < -10){
    　　　　　　　　if(this.index !== this.icon.length-1) this.index += 1;
    　　　　　　}
    　　　　}
    　　　　this.icon[this.index].className = 'curr';
    　　　　this.slider.className = 'cnt f-anim';
    　　　　this.slider.style.left = -this.index*600 + 'px';
    　　}
    　　//解绑事件
    　　this.slider.removeEventListener('touchmove',this,false);
    　　this.slider.removeEventListener('touchend',this,false);
    },

手指离开屏幕后，所执行的函数。这里先判断手指停留屏幕上的时间，如果时间太短，则不执行该函数。再判断手指是左滑动还是右滑动，分别执行不同的操作。最后很重要的一点是移除touchmove,touchend绑定事件。

### 源代码：

    <!DOCTYPE html>
    <html>
    <head>
    <meta http-equiv="Content-Type" Content="text/html; charset=utf-8;">
    <title>移动端触摸滑动</title>
    <meta name="author" content="rainna" />
    <meta name="keywords" content="rainna's js lib" />
    <meta name="description" content="移动端触摸滑动" />
    <meta name="viewport" content="target-densitydpi=320,width=640,user-scalable=no">
    <style>
    *{margin:0;padding:0;}
    li{list-style:none;}

    .m-slider{width:600px;margin:50px 20px;overflow:hidden;}
    .m-slider .cnt{position:relative;left:0;width:3000px;}
    .m-slider .cnt li{float:left;width:600px;}
    .m-slider .cnt img{display:block;width:100%;height:450px;}
    .m-slider .cnt p{margin:20px 0;}
    .m-slider .icons{text-align:center;color:#000;}
    .m-slider .icons span{margin:0 5px;}
    .m-slider .icons .curr{color:red;}
    .f-anim{-webkit-transition:left .2s linear;}
    </style>
    </head>

    <body>
    <div class="m-slider">
        <ul class="cnt" id="slider">
            <li>
                <img src="http://imglf1.ph.126.net/qKodH3sZoVbPalKFtHS9mw==/6608946691259322175.jpg">
                <p>20140813镜面的世界，终究只是倒影。看得到你的身影，却触摸不到你的未来</p>
            </li>
            <li>
                <img src="http://imglf1.ph.126.net/40-jqH_j6EoCWnZOixY2pA==/4798022453110310215.jpg">
                <p>20140812锡林浩特前往东乌旗S101必经之处，一条极美的铁路。铁路下面是个小型的盐沼，淡淡的有了一丝天空之境的感觉。可惜在此玩了一个小时也没有看见一列火车经过，只好继续赶往东乌旗。</p>
            </li>
            <li>
                <img src="http://imglf0.ph.126.net/Jnmi2y51zVdjKAYlibtpFw==/3068640196117481166.jpg">
                <p>20140811水的颜色为什么那么蓝，我也纳闷，反正自然饱和度和对比度拉完就是这个颜色的</p>
            </li>
            <li>
                <img src="http://imglf1.ph.126.net/79GPsjhwiIj8e-0nP5MsEQ==/6619295294699949331.jpg">
                <p>海洋星球3重庆天气热得我想卧轨自杀</p>
            </li>
            <li>
                <img src="http://imglf1.ph.126.net/40-jqH_j6EoCWnZOixY2pA==/4798022453110310215.jpg">
                <p>以上这些作品分别来自两位设计师作为观者，您能否通过设计风格进行区分</p>
            </li>
        </ul>
        <div class="icons" id="icons">
            <span class="curr">1</span>
            <span>2</span>
            <span>3</span>
            <span>4</span>
            <span>5</span>
        </div>
    </div>

    <script>
    var slider = {
        //判断设备是否支持touch事件
        touch:('ontouchstart' in window) || window.DocumentTouch && document instanceof DocumentTouch,
        slider:document.getElementById('slider'),

        //事件
        events:{
            index:0,     //显示元素的索引
            slider:this.slider,     //this为slider对象
            icons:document.getElementById('icons'),
            icon:this.icons.getElementsByTagName('span'),
            handleEvent:function(event){
                var self = this;     //this指events对象
                if(event.type == 'touchstart'){
                    self.start(event);
                }else if(event.type == 'touchmove'){
                    self.move(event);
                }else if(event.type == 'touchend'){
                    self.end(event);
                }
            },
            //滑动开始
            start:function(event){
                var touch = event.targetTouches[0];     //touches数组对象获得屏幕上所有的touch，取第一个touch
                startPos = {x:touch.pageX,y:touch.pageY,time:+new Date};    //取第一个touch的坐标值
                isScrolling = 0;   //这个参数判断是垂直滚动还是水平滚动
                this.slider.addEventListener('touchmove',this,false);
                this.slider.addEventListener('touchend',this,false);
            },
            //移动
            move:function(event){
                //当屏幕有多个touch或者页面被缩放过，就不执行move操作
                if(event.targetTouches.length > 1 || event.scale && event.scale !== 1) return;
                var touch = event.targetTouches[0];
                endPos = {x:touch.pageX - startPos.x,y:touch.pageY - startPos.y};
                isScrolling = Math.abs(endPos.x) < Math.abs(endPos.y) ? 1:0;    //isScrolling为1时，表示纵向滑动，0为横向滑动
                if(isScrolling === 0){
                    event.preventDefault();      //阻止触摸事件的默认行为，即阻止滚屏
                    this.slider.className = 'cnt';
                    this.slider.style.left = -this.index*600 + endPos.x + 'px';
                }
            },
            //滑动释放
            end:function(event){
                var duration = +new Date - startPos.time;    //滑动的持续时间
                if(isScrolling === 0){    //当为水平滚动时
                    this.icon[this.index].className = '';
                    if(Number(duration) > 10){     
                        //判断是左移还是右移，当偏移量大于10时执行
                        if(endPos.x > 10){
                            if(this.index !== 0) this.index -= 1;
                        }else if(endPos.x < -10){
                            if(this.index !== this.icon.length-1) this.index += 1;
                        }
                    }
                    this.icon[this.index].className = 'curr';
                    this.slider.className = 'cnt f-anim';
                    this.slider.style.left = -this.index*600 + 'px';
                }
                //解绑事件
                this.slider.removeEventListener('touchmove',this,false);
                this.slider.removeEventListener('touchend',this,false);
            }
        },
        
        //初始化
        init:function(){
            var self = this;     //this指slider对象
            if(!!self.touch) self.slider.addEventListener('touchstart',self.events,false);    //addEventListener第二个参数可以传一个对象，会调用该对象的handleEvent属性
        }
    };

    slider.init();
    </script>
    </body>
    </html>
