

写一些webrtc转rtmp


docker cp srs.sdk.js 15b5fe891a7b:/usr/local/srs/objs/nginx/html/demos/js/
docker cp test.html 15b5fe891a7b:/usr/local/srs/objs/nginx/html/demos/

https://121.37.103.111/demos/test.html?autostart=true

音频约束参数
volume 音量约束
sampleRate: 采样率
sampleSize: 采样大小，采样的位数
echoCancellation: 回音消除
autoGaincontrol： 增加音量
noiseSuppression: 降噪
latency ： 延迟大小
channelCount: 切换声道
deviceID: 多个音频输入输出设备的进行切换
groupId: 同一个物理设备，是一个分组，但是输入和输出的id不一样

