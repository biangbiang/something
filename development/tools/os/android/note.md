安卓开发笔记
============

### How to solve Exception raised during rendering: java.lang.System.arraycopy([CI[CII)V in Android

在布局添加控件手动添加还是拖的添加，添加edittext后布局就不好用，其他控件好用，然后就说下面这段话

Exception raised during rendering: java.lang.System.arraycopy([CI[CII)V

Exception details are logged in Window > Show View > Error Log

Check the "Android version to use when rendering layouts" and make sure you're not using a version that ends in "W" for Android Wear (e.g. API 20: Android 4.4W). I don't believe Wear supports EditText.

![](http://biang.io/biangpic/blog/3e95e6202584854a0288a2797ab1cf52.png)

修改选择不同的API就好了，降低版本即可

---

### Rendering Problems in Intellij 13.1.3,there is only API 20 in “Android version to use when rendering layouts in the IDE”

I read this answer Rendering issue for Android with Intellij 13.1.3 ,but there is only API 20,can not set lower version

![](http://biang.io/biangpic/blog/2310f89354ff50643073b94aa36c4142.jpg)

my project AndroidManifest.xml

    <?xml version="1.0" encoding="utf-8"?>
    <manifest
    package="com.example.myapp"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionCode="1"
    android:versionName="1.0">

    <uses-sdk
        android:minSdkVersion="9"
        android:targetSdkVersion="16"/>
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name">
        <activity
            android:name="MyActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>

did I miss something?


You don't have the other SDKs.

To download them, open the SDK manager from Android Studio, and download Android 4.4.2 (API 19).

---

### Download interrupted: Connection to https://dl-ssl.google.com refused  

这个可能是网络问题，国内连google服务器经常连不上。

尝试用下面办法试下：

1. 上图SDK Manager 的 Tools ->Options打开SDK Manager的Settings，选中“Force https://… sources to be fetched using http://…”，强制使用http协议。

  ![](http://biang.io/biangpic/blog/167f6f108c6ad3d1e6dbfe0225d2ff05.jpg)

  ![](http://biang.io/biangpic/blog/8acef821487d293f1f05a727e70137d5.jpg)

2. 改hosts文件。

  Windows在C:\WINDOWS\system32\drivers\etc目录下，

  Linux用户打开/etc/hosts文件，   

  打开文件后添加以下内容：

        #android更新

        203.208.46.146 dl.google.com

        203.208.46.146 dl-ssl.google.com

---

### 如何在 Mac 下搭建 Android Build 环境？

官方文档：http://source.android.com/source/initializing.html#setting-up-a-mac-os-x-build-environment

1. Mac的默认文件格式是不区分大小写的，所以要创建一个区分大小写的分区。

  也就是文中的：

  hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 40g ~/android.dmg

  大小为40G，位置是用户目录，名字为android.dmg.

2. 加载分区，记得我当时是双击解决。文中的方式是在~/.bash_profile 中加入以下代码：

  function mountAndroid { hdiutil attach ~/android.dmg -mountpoint /Volumes/android; }

  然后在终端执行mountAndroid

3. 接下来不同的源码版本编译环境不同，我建议你直接编译Master.

  要求环境是： MacOS 10.8 (Mountain Lion), along with Xcode 4.5.2 and Command Line Tools(不过我当时环境的确是这个) JDK7.

4. Installing required packages这个按说的做就行了。

5. 下载源码，也有文档

6. 编译。
