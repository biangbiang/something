git查看某个文件的修改历史
=========================

有时候在比对代码时，看到某些改动，但不清楚这个改动的作者和原因，也不知道对应的BUG号，也就是说无从查到这些改动的具体原因了～

【注】：某个文件的改动是有限次的，而且每次代码修改的提交都会有commit描述，我们可以从这里进行入手；

### 一、切换到目录

首先切换到要查看的文件所在的目录：

    cd packages/apps/Mms/src/com/android/mms/ui/

### 二、git log --pretty

然后使用下面的命令可列出文件的所有改动历史，注意，这里着眼于具体的一个文件，而不是git库，如果是库，那改动可多了去了～

    git log --pretty=oneline 文件名

如：

    root@ubuntu:android_src/packages/apps/Mms/src/com/android/mms/ui# git log --pretty=oneline MessageItem.java 
    27209385caf678abe878375a470f4edd67a2d806 fix to process force close when empty address contained in card
    0e04b16f1dad7dc0a36e2235f7337bc656c365c7 display for 1970-1-1
    e4abf3a213197491e0855e101117b59b5dc0160d HREF#13954 receive, store, and display wap push
    356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e HREF#16265_uim_show_time_error
    350f9d34c35ab50bdb4b2d43fb3ff9780e6c73fa fix xxxx
    715e32f97bd9d8ce4b5ba650b97ba4b137150456 Fix ANR from calling Contact.get()
    fd8357ff5febab0141e1beb8dd3b26f70416b108 Fix missing From field
    d130e2e6dc448fd80ecb70f0d31e3affb9888b9a fix bug 2112925: don't display zip file garbage content in MMS.
    0e19f738c114f86d0d88825ee48966015fb48b6d Don't always show sent timestamp
    52f854cbb75e8f9975c7e33216b828eb2f981095 Don't show Anonymous as the MMS sender
    331864544ec51ba6807fc5471cc6d537b7fef198 add search capability
    33a87f96f8c625aa10131a77a3968c97c4ec5a62 Remove all references to ContactInfoCache except those in Contact.
    70c73e05a792832aa28da751cdaf3fa83a7b8113 Begin moving all conversation data behind a data model with a cache.
    48da875f1beea835c6771977e5bd8a9aa3d4bc10 Begin adding UI unit tests to the Mms app.
    66dde9460badebf8e740275cabde9cca256006eb Stop requiring a Context to be passed in to ContactInfoCache.
    591d17e9a51bb9f829d6860dc7aa0bad25062cd5 auto import from //branches/cupcake_rel/...@138607
    72735c62aba8fd2a9420a0f9f83d22543e3c164f auto import from //depot/cupcake/@135843
    892f2c5bf965b1431ae107b602444a93f4aad4a3 auto import from //depot/cupcake/@135843
    153ae99e0a7d626a24d61475eeb133249deb448c auto import from //depot/cupcake/@132589
    abd7b2d90f7491075f1daba4b4cccdfc82f8ddd1 auto import from //depot/cupcake/@137055
    59d72c57ce9c319b6cd43ce2ab36b7076c9e821f auto import from //branches/cupcake/...@132276
    44cea74dc55e2459262d0d765ef4a69267dd09b0 auto import from //branches/cupcake/...@131421
    0f236f55349f070ac94e12cca963847173393da8 Code drop from //branches/cupcake/...@124589
    8eed706474910ccb978acda03e85d3261037da6e Initial Contribution

### 三、git show

如上所示，打印出来的就是针对文件MessageItem.java的所有的改动历史，每一行最前面的那一长串数字就是每次提交形成的哈希值，接下来使用git show即可显示具体的某次的改动的修改～

    git show 356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e

结果如下：

    root@ubuntu:/android_src/packages/apps/Mms/src/com/android/mms/ui# git show 356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e
    commit 356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e
    Author: 某某某 <某某某的邮箱>
    Date:   Thu Jan 6 01:50:31 2011 +0800

        修改的描述（是该代码commit时所填）
        
        Signed-off-by: 某某某 <某某某的邮箱>

    diff --git a/src/com/android/mms/ui/MessageItem.java b/src/com/android/mms/ui/MessageItem.java
    index 0a0c4b7..55c3b27 100644
    --- a/src/com/android/mms/ui/MessageItem.java
    +++ b/src/com/android/mms/ui/MessageItem.java
    +
    + 列出具体的改动
    -
    -

这样就可以知道是谁做了修改，以及具体的修改代码～

那接下来不管是直接去找他交流还是研究代码，都有依据了～

