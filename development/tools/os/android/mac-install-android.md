在mac上搭建Android开发环境
==========================

首先我们需要知道开发安卓的环境由以下几个组件组成：Java虚拟机JDK、Eclipse、Eclipse插件ADT(Android Developer Tool)和Android开发包SDK。下面就和大家一起学习如何来快速的，适合新手的搭建方式吧！

---

## 方法/步骤

1. 因为系统自带jdk，所以就不用在下载了。可以在终端输入java -version查看其结果。

  ![](http://biangbiangpic.b0.upaiyun.com/blog/6c0d03039ef698b1294e6d504d481602.jpg)

2. 然后去android的官方网站下载ADT工具

  http://developer.android.com/sdk/index.html
  
  可能之前很多人的教程中都说需要SDK+Eclipse，还有Eclipse插件
  
  但是现在官方把他们全部整合在一起了。
  
  官方的原话“With a single download, the ADT Bundle includes everything you need to begin developing apps”
  
  仅仅下载这个ADT（安卓开发工具包）就包含了你开发所需要的所有东西！
  
  实际就是官方为了方便大家开发，给我们进行了打包吧。

  ![](http://biangbiangpic.b0.upaiyun.com/blog/db92c399c86b145b98e6d4348dcc6a78.jpg)

3. 解压下载完的ADT包，里面有两个文件夹，一个eclipse一个SDK

  ![](http://biangbiangpic.b0.upaiyun.com/blog/f5ae91614dd3279da577876ce81368ca.jpg)

4. 继续跟着官方文档走，建议将其解压在家目录下的Development目录下，我没有Development目录，但是作为一个菜鸟，我还是决定完全照着官方的意思走，所以我新建了一个Development，然后把刚刚自动解压的eclipse和sdk都移了进来（如何操作？我是使用的终端mkdir Development 然后mv进来的）

  ![](http://biangbiangpic.b0.upaiyun.com/blog/1c9aca7b226898b11265f508cbac1d94.jpg)

5. 进入到eclipse目录，然后启动它！激动人心的时刻！

  ![](http://biangbiangpic.b0.upaiyun.com/blog/9a37c353e7d257b292ed81a241855256.jpg)

  ![](http://biangbiangpic.b0.upaiyun.com/blog/43285cd6e77d3262c9d528aa249676b0.jpg)

6. 现在似乎一起都准备就绪了，可以创建自己的第一个app了！

  ![](http://biangbiangpic.b0.upaiyun.com/blog/ac18dbc8b216b7aa676533f082378e77.jpg)
