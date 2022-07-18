# RTC安卓SDK接入文档

## 需求说明

1. 基于libwebrtc安卓端SDK，实现移动端音视频通话Demo应用。
2. 优先实现一对一的音频方案验证。
3. 实现语音通话功能，包括：选择服务器地址，发起通话，接受通话，结束退出通话。
4. 显示链接日志信息，包括：链接过程的日志，收到的sdp信息，candidate信息，用于调试问题。

## SDK导入方法

1. Android Studio 引入AAR的方法：

Gradle 7.0+ 设置aar路径，将libwebrtc.aar放在app/libs下，dependencies 增加：

```gradle
方式一
    implementation files('libs/libwebrtc.aar')

方式二
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])

```

2. Gradle 7.0之前的旧版本设置aar路径：

```gradle
方式一
    repositories {
        flatDir {
            dirs 'libs'
        }
    }
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    implementation(name: 'libwebrtc', ext: 'aar')

方式二

    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
```

3. 当需要引用webrtc官方的最新版本库时（需翻墙），可以使用以下方式：

在项目 settings.gradle 中增加 jcenter 和 google 的仓库

```gradle
pluginManagement {
    repositories {
        gradlePluginPortal()
        google()
        mavenCentral()
        jcenter()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        jcenter()
    }
}

在build.gradle(:app)中的 dependencies 中添加：

dependencies {
    implementation 'org.webrtc:google-webrtc:1.0.+'
}

sync后在 External Libraries查看时，当前版本应为1.0.32006

```

4. 权限申请开放，在安卓6.0以上，至少需要权限：

CAMERA、RECORD_AUDIO、WRITE_EXTERNAL_STORAGE等

## 系统背景介绍

简要介绍需要必须了解的背景知识和名词解释，方便后续说明SDK API的使用。

signaling
ICE server
SFU server

## SDK关键类功能说明

介绍Webrtc一些关键类，下面的例子供参考，具体调用根据需要编写。

1. PeerConnectionFactory

webrtc核心类，用于创建其他关键类PeerConnection

```java
PeerConnectionFactory.initialize(
        PeerConnectionFactory.InitializationOptions.builder(
                getApplicationContext()).createInitializationOptions());

PeerConnectionFactory factory = PeerConnectionFactory.builder().createPeerConnectionFactory();
```

2. VideoCapturer
视频捕捉器的一个顶级接口，它的的子接口为CameraVideoCapturer，封装了安卓相机的使用方法，可以用于获取设备相机数据，切换摄像头，获取摄像头数量等。该对象的创建如下。

下面是一个轮询捕获摄像头的例子供参考

```java
    private CameraVideoCapturer createVideoCapture(Context context) {
        CameraEnumerator enumerator;
        if (Camera2Enumerator.isSupported(context)) {
            enumerator = new Camera2Enumerator(context);
        } else {
            enumerator = new Camera1Enumerator(true);

            final String[] deviceNames = enumerator.getDeviceNames();

            for (String deviceName : deviceNames) {
                if (enumerator.isFrontFacing(deviceName)) {
                    CameraVideoCapturer videoCapturer = enumerator.createCapturer(deviceName, null);

                    if (videoCapturer != null) {
                        return videoCapturer;
                    }
                }
            }

            for (String deviceName : deviceNames) {
                if (!enumerator.isFrontFacing(deviceName)) {
                    CameraVideoCapturer videoCapturer = enumerator.createCapturer(deviceName, null);
                    if (videoCapturer != null) {
                        return videoCapturer;
                    }
                }
            }
            return null;
        }
    }
```

3. VideoSource/VideoTrack
VideoSource为视频源，通过核心类PeerConnectionFactory创建，VideoTrack是对VideoSource的包装，可以方便的将视频源在本地进行播放，添加到MediaStream中进行网络传输。

```java
    CameraVideoCapturer mVideoCapturer = createVideoCapture(this);
    VideoSource videoSource = mPeerConnectionFactory.createVideoSource(mVideoCapturer);
    VideoTrack mVideoTrack = mPeerConnectionFactory.createVideoTrack("videtrack", videoSource);

```

4. AudioSource/AudioTrack
AudioSource/AudioTrack和上面的VideoSource/VideoTrack类似，是对音频的获取和处理了，用PeerConnectionFactory创建。

```java
AudioSource audioSource = mPeerConnectionFactory.createAudioSource(new MediaConstraints());
AudioTrack mAudioTrack = mPeerConnectionFactory.createAudioTrack("audiotrack", audioSource);
```

AudioSource 创建的时候需要传入MediaConstraints这个对象的实例，其用于对媒体的一些约束限制，创建的时候可以直接使用默认的。也可以用下列建议参数，完整的参数列表见官方文档

```java
MediaConstraints audioConstraints = new MediaConstraints();
//回声消除
audioConstraints.mandatory.add(new MediaConstraints.KeyValuePair("googEchoCancellation", "true"));
//自动增益 
audioConstraints.mandatory.add(new MediaConstraints.KeyValuePair("googAutoGainControl", "true"));
//高音过滤 
audioConstraints.mandatory.add(new MediaConstraints.KeyValuePair("googHighpassFilter", "true"));
//噪音处理
audioConstraints.mandatory.add(new MediaConstraints.KeyValuePair("googNoiseSuppression", "true"));
```

5. MediaStream
音视频的媒体流，通过PeerConnectionFactory创建，用于PeerConnection通过网络传输发送给另一方。在媒体流传输之前，需要将前面获取的VideoTrack和AudioTrack添加进去。

```java
MediaStream mMediaStream = mPeerConnectionFactory.createLocalMediaStream("localstream");
mMediaStream.addTrack(mVideoTrack);
mMediaStream.addTrack(mAudioTrack);
```

6. PeerConnection (重要)
用于p2p网络传输，双方信令的交换。webrtc是基于p2p的，因此在双方通信之前需要服务器帮助传递信令，并且需要添加STUN和TURN服务器网络穿透。在双方通道打开之后就可以将媒体流发送给另一方。下面是PeerConnection的创建，并将媒体流添加到其中用于网络传输。

```java
PeerConnection peerConnection = mPeerConnectionFactory.createPeerConnection(iceServers, pcConstraints, this);
peerConnection.addStream(mMediaStream);
```

iceServers是连接到外网和网络穿透用的，可以添加STUN和TURN服务器。

constraints是一个MediaConstrains的实例。可以包含offerToRecieveAudio和offerToRecieveVideo。

```java
  constraints = new MediaConstraints();
  constraints.optional.add(new MediaConstraints.KeyValuePair("offerToReceiveAudio", "true"));
  constraints.optional.add(new MediaConstraints.KeyValuePair("offerToReceiveVideo", "false"));
```

observer是一个PeerConnectionObserver的实例，对PeerConnection的一些连接状态的监听。

## 客户端时序

![peer-connection.png](peer-connection.png)

上图是主要关键性 API 的调用流程，不是完全的流程，编程需参考 WebRTC 的文档注释。这里针对时序图中的一些情况做具体说明：

先进入房间的用户是发起方（Indicator），后进入房间的用户是参与者（Participant）。本次DemoApp可以先不用考虑多房间，只演示单个房间的情况。

add audio & video tracks 不是连接流程中的关键步骤，也可以在 ICE 流程之后再执行。
在 SetLocalDescription 执行成功后，协商 SDP 和 ICE candidate 的流程便会同时开始。

通话双方均与选定的 ICE 服务器连接成功后，即可开始相互推流。在本次实现中 SFU 服务器同时充当 ICE 服务器的角色。

## SDP交换逻辑详细说明

WebRTC 使用 PeerConnection 这个类创建连接，它包含两个生成 SDP 的方法，分别是 CreateOffer() 和 CreateAnswer()，CreateOffer()由会话发起方调用生成 offerSdp 并发送到信令服务器；CreateAnswer()在应答方在收到信令服务器消息后被调用生成 answerSdp，然后也发送回信令服务器。

PeerConnection 还包含两个设置 SDP 的方法，分别是 SetLocalDescription() 和 SetRemoteDescription()，前者用来设置本地（自己）的 localSdp，后者用来设置接收到的对端 remoteSdp。对于会话发起方而言，localSdp 是 offerSdp，remoteSdp 是 answerSdp；对于应答方而言，localSdp 是 answerSdp，而 remoteSdp 是接收到的 offerSdp。

上述四个方法的有序组合即可构成 WebRTC 设备之间的 SDP 交换过程。其实这也是一种协商过程，因为应答方需要根据发起方提供的 SDP（ICE 服务器信息、音视频编码信息等）决定如何回复。因为与后续的「连接流程」章节的内容有重合，这里只是简单地给出示意图：

![sdp.png](sdp.png)

信令交换###

播放媒体流###

Demo###

见附带的WebrtcDemo.zip

## 视频推流过程

https://webrtc.mthli.com/media/video-outbound/

视频的推流大致可以分为采集、渲染、编码和发送四个过程。

将视频流从相机显示到视图的步骤是，

创建并初始化 PeerConnectionFactory
创建一个使用设备摄像头的 VideoCapturer 实例
从 Capturer 创建一个 VideoSource
从源创建 VideoTrack
使用 SurfaceViewRenderer 视图创建视频渲染器并将其添加到 VideoTrack 实例

https://vivekc.xyz/getting-started-with-webrtc-for-android-daab1e268ff4

### 采集
### 




## 参考资料

W3C WebRTC 1.0
https://www.w3.org/TR/webrtc/

https://w3c.github.io/webrtc-stats/