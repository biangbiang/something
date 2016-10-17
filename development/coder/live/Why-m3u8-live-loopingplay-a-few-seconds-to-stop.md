# m3u8的直播为什么循环播放/播放几秒就停止了

首先，需要了解一下，m3u8直播的原理：假设直接地址是live.m3u8

该文件里的直播地址格式是

    #EXTM3U
    #EXT-X-VERSION:3
    #EXT-X-TARGETDURATION:6
    #EXT-X-MEDIA-SEQUENCE:0
    #EXTINF:4.797000,
    index0.ts
    #EXTINF:4.396000,
    index1.ts
    #EXTINF:5.297000,
    index2.ts

上面文件中的节点作用是：

    #EXTM3U                     m3u文件头，必须放在第一行
    #EXT-X-VERSION                     版本号
    #EXT-X-MEDIA-SEQUENCE       第一个TS分片的序列号
    #EXT-X-TARGETDURATION       每个分片TS的最大的时长
    #EXT-X-ALLOW-CACHE          是否允许cache
    #EXT-X-ENDLIST              m3u8文件结束符
    #EXTINF                     extra info，分片TS的信息，如时长，带宽等

当播放器第一次请求会加载这里的：index0.ts,index1.ts,index2.ts来播放，因为该m3u8里没有设置结束节点。所以播放器判断出该文件为直播文件，则在(#EXT-X-TARGETDURATION)设置的时间后重新加载live.m3u8，此时因为是直播， 所以live.m3u8里应该会自动增加一个新的ts文件流，即live.m3u8变成如下的格式：

    #EXTM3U
    #EXT-X-VERSION:3
    #EXT-X-TARGETDURATION:6
    #EXT-X-MEDIA-SEQUENCE:1
    #EXTINF:4.797000,
    index1.ts
    #EXTINF:4.396000,
    index2.ts
    #EXTINF:5.297000,
    index3.ts

从上面的可以看出，index0.ts没了，多了一个index3.ts。播放器会把index3.ts添加到播放列表里。这样循环下去。则会不停的进行直播的动作。

了解了以上的原理后。就容易分析原因了。

循环播放或播放一定时间后停止，都是因为播放器没有请求到新的ts文件导致。可能是使用了cdn缓存机制导致，或服务器端设置了缓存机制导致。
