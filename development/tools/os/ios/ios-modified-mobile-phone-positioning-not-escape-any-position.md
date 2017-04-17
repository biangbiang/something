# iOS修改手机定位(非越狱任意位置)

利用开发者的一些调试功能，我们可以修改非越狱的苹果手机定位，模拟任意位置。

经测试，此方法仅限开发者调试使用，并不能长时间修改手机定位。

1. 首先需要了解一些坐标系的知识

  iOS，原生坐标系为 WGS-84

  高德以及国内坐标系：GCS-02

  百度的偏移坐标系：BD-09

  这些先了解，下面需要用到转换

2. 从高德地图拾取网页上找到要模拟的地点坐标，这里我测试使用 九寨沟

  http://lbs.amap.com/console/show/picker 

  得到高德坐标：103.627229,32.755169

  ![](http://biangbiangpic.b0.upaiyun.com/blog/d94c4a7ad11a29aa2634c30beacaf7ab.jpg)

  由于此坐标在手机上地图显示时，会有偏移误差，所以需要转换成 WGS-84苹果用；

  在网上找算法转换后：

  得到九寨沟坐标：33.144513 103.910688

  在后面的.gpx文件中就放上转换后的坐标，以后修改此处来模拟其他位置

3. 用Xcode创建一个工程FakeGPS

  此时在外面新建立一个 JZG.gpx 的 XML 文件 文件信息如下，然后导入工程

    <?xml version="1.0" encoding="UTF-8" ?>
    <gpx version="1.1"
         creator="GMapToGPX 6.4j - http://www.elsewhere.org/GMapToGPX/"
         xmlns="http://www.topografix.com/GPX/1/1"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd">
          <wpt lat="33.144513" lon="103.910688">
             <name>chengdu</name>
             <cmt>九寨沟</cmt>
             <desc>九寨沟</desc>
          </wpt>
    </gpx>

4. 在工程 Produce---Scheme---Eidt Scheme---Options ---

  在 Default location 里面选择导入的 JZG ；就是上面JZG.gpx的文件名，对应上述地理位置；

  OK, 真机运行FakeGPS工程；

  ![](http://biangbiangpic.b0.upaiyun.com/blog/f49a96faf3576c55f4b6113f115bc05d.jpg)

5. 在手机上运行FakeGPS工程，然后打开 手机上的高德地图app，定位，即可看到当前位置已模拟在目的地了，

  同样查看微信定位，也是要模拟的位置了；

  ![](http://biangbiangpic.b0.upaiyun.com/blog/981422d85c9fcd0106769a86579eced6.jpg)

6. 实测上面的位置并不会永久修改，当模拟位置的app退出或是一些其他原因模拟的位置就会不在起作用；

  也印证了该功能仅用于开发者调试使用，不过对于一些短时间的修改位置测试使用场景够用了

7. 演示工程的DEMO下载

  https://github.com/cocoajin/TDDDemo/tree/master/FakeGPS

参考：

  坐标转换 http://blog.csdn.net/jijiji000111/article/details/52468042
