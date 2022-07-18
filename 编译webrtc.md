

the package you're trying to download has itself been deprecated since January 2020 (with the exception of a security update in mid 2020):https://groups.google.com/g/discuss-webrtc/c/lQJ6bPILCAs

https://groups.google.com/g/discuss-webrtc/c/Ozvbd0p7Q1Y/m/M4WN2cRKCwAJ

https://webrtc.org.cn/20190419_tutorial3_webrtc_android/
https://webrtc.org.cn/can-you-hear-me/
https://webrtc.org.cn/getusermedia-common-error/
https://webrtc.org.cn/20180825-webrtc-server-p2p/
https://webrtc.org.cn/dont-mix-media/

https://webrtc.org.cn/webrtc-tutorial-1-setup-signaling/
https://webrtc.org.cn/webrtc-tutorial-2-signaling-stun-turn/
https://webrtc.org.cn/20190419_tutorial3_webrtc_android/



编译
https://www.sobyte.net/post/2022-03/how-to-compile-native-webrtc-library-from-source-for-android/
https://blog.csdn.net/newchenxf/article/details/116302993
https://medium.com/@abdularis/how-to-compile-native-webrtc-from-source-for-android-d0bac8e4c933
https://blog.csdn.net/u013075460/article/details/124442680
https://groups.google.com/g/discuss-webrtc/c/lQJ6bPILCAs
https://wenku.baidu.com/view/904051862b4ac850ad02de80d4d8d15abe2300ff.html
https://www.jianshu.com/p/30aef734eefa
https://medium.com/@abdularis/how-to-compile-native-webrtc-from-source-for-android-d0bac8e4c933
https://stackoverflow.com/questions/72103146/how-to-fix-failed-to-resolve-org-webrtcgoogle-webrtc
https://stackoverflow.com/questions/69252071/replacing-jcenter-with-mavencentral-causes-error-in-webrtc
https://webrtc.github.io/webrtc-org/native-code/android/

https://blog.csdn.net/epubcn/article/details/86468538
https://juejin.cn/post/6956379281907777572

编写代码调试
https://www.writebug.com/git/Blair_78/AudioANDVideoScriptKilling/src/branch/master/TestWebRTC/app
https://blog.csdn.net/weixin_33693070/article/details/91450387
https://www.jianshu.com/p/b4dbca042678
https://juejin.cn/post/6844904162015051789
https://juejin.cn/post/6960844935234846734


基于SRS的WebRTC直播流的Android端实现
https://www.jianshu.com/p/37519ae3e2f6
https://blog.csdn.net/kyl282889543%2Farticle%2Fdetails%2F106816541
https://blog.csdn.net/x277151676%2Farticle%2Fdetails%2F114583609
https://juejin.cn/post/7009263889221156872

https://www.jianshu.com/p/82e615aede15



https://vivekc.xyz/getting-started-with-webrtc-part-4-de72b58ab31e



Android Studio 引入AAR的方式
千次阅读
2022-03-07 14:23:12
Gradle 7.0+ 设置aar路径

aar放在app/libs下
dependencies 增加

方式一
    implementation files('libs/libwebrtc.aar')

方式二
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])


之前版本Gradle设置aar路径
方式一
repositories {
    flatDir {
        dirs 'libs'
    }
}
implementation fileTree(dir: 'libs', include: ['*.jar'])

implementation(name: 'AarLib_V1.0.0', ext: 'aar')

方式二

    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])

https://support.huaweicloud.com/csdk-rtc/rtc_05_0003.html

https://github.com/pchab/AndroidRTC

https://github.com/CharonChui/AndroidNote/blob/master/VideoDevelopment/Android%20WebRTC%E7%AE%80%E4%BB%8B.md

https://github.com/CharonChui/AndroidNote/blob/master/VideoDevelopment/Android%20WebRTC%E7%AE%80%E4%BB%8B.md

https://vivekc.xyz/getting-started-with-webrtc-part-4-de72b58ab31e

https://vivekc.xyz/peerconnection-getting-started-with-webrtc-part-2-e426b08d1028

https://webrtc.mthli.com/

https://www.cnblogs.com/yanwei-wang/p/12128223.html

https://blog.csdn.net/qq_35054800/article/details/78647545

https://download.csdn.net/download/qq_35054800/10135721


基于WebRtc实现安卓视频一对一聊天
https://www.cnblogs.com/yanwei-wang/p/12128223.html