https://mp.weixin.qq.com/s/xWe6f9WRhtwnpJQ8SO0Eeg

https://mp.weixin.qq.com/s/CM2h99A1e_masL5sjkp4Zw

https://ossrs.net/releases/

安卓
https://github.com/ossrs/flutter_live

索引
https://github.com/ossrs/srs/wiki/v4_CN_Docs

服务器端

服务器端+webRTC
https://github.com/ossrs/srs/wiki/v4_CN_WebRTC

https://github.com/ossrs/srs/issues/307

https://github.com/ossrs/srs/wiki/v4_CN_HTTPApi#webrtc-publish
https://github.com/ossrs/srs/wiki/v4_CN_HTTPApi#webrtc-play

关于Candidate


UDP 8000端口检测
https://github.com/ossrs/srs/issues/2843


Web端
https://mp.weixin.qq.com/s?__biz=MzA4NTQ3MzQ5OA==&mid=2649004518&idx=1&sn=c2ea8d4ac9cf4e28976c1c3342b94936&chksm=87c75853b0b0d1459423b429309dc800dee8e3894df55db92f90b21908c821f22abbf8c77847&scene=21#wechat_redirect

web端集成demo
https://github.com/ossrs/srs/blob/develop/trunk/research/players/js/srs.sdk.js

https://github.com/ossrs/srs/tree/develop/trunk/3rdparty/signaling/www/demos

# 信令服务 signaling
https://mp.weixin.qq.com/s?__biz=MzA4NTQ3MzQ5OA==&mid=2649004531&idx=1&sn=c710d20de6c1e9e1e7fdb966546f4435&chksm=87c75846b0b0d1503f68469762bd422af4c19e18e0308022e95967a5a4252ea3ff1fb1a69daf&scene=21#wechat_redirect

https://github.com/ossrs/srs/issues/307

安卓端
https://github.com/ossrs/flutter_live/tree/main/example
性能
https://mp.weixin.qq.com/s/r2jn1GAcHe08IeTW32OyuQ
Note: 在音视频服务器的性能优化中，延迟是必须要考虑的一个因素，特别是在RTC服务器，性能优化不能以增大延迟为方法。

SRS的基准是并发流，比如使用srs-bench[17]推流可以获得支持的最高推流（发布）并发，和最高拉流（播放）并发。压测工具一般读取文件，可以选择典型的业务场景，录制成样本文件，这样压测可以尽量模拟线上场景。

https://github.com/ossrs/srs-bench/tree/feature/rtc#usage

扩展并发量

https://mp.weixin.qq.com/s/pd9YQS0WR3hSuHybkm1F7Q

https://mp.weixin.qq.com/s/pd9YQS0WR3hSuHybkm1F7Q

其他
https://space.bilibili.com/430256302/channel/collectiondetail?sid=44177&ctype=0
https://webrtc.mthli.com/

configure分析
https://www.xianwaizhiyin.net/?p=991
