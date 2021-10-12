# 前端实现H5浏览器扫码功能

![banner](http://127.0.0.1:8080/images/banner.png)

背景介绍、实现效果、技术简介、代码实现、总结等5部分组成

## 背景

云盘有邀请成员加入云盘功能，成员可以通过管理员提供的企业二维码加入云盘。
移动客户端APP已经实现成员扫码加入云盘的功能，现在云盘H5也需要对标APP，实现用户扫码加入云盘功能。

## 实现效果

用户在申请加入云盘的页面进行如下操作：
选择二维码加入方式
获取浏览器访问摄像头权限
扫码识别
跳转进入申请页面


## 技术简介

### WebRTC API

**WebRTC** (Web Real-Time Communications) 是一项实时通讯技术，它允许网络应用或者站点，在不借助中间媒介的情况下，建立浏览器之间点对点（Peer-to-Peer）的连接，实现视频流和（或）音频流或者其他任意数据的传输。WebRTC包含的这些标准使用户在无需安装任何插件或者第三方的软件的情况下，创建点对点（Peer-to-Peer）的数据分享和电话会议成为可能。

**三个主要接口**：
* MediaStream：能够通过设备的摄像头及话筒获得视频、音频的同步流
* RTCPeerConnection：是WebRTC用于构建点对点之间稳定、高效的流传输的组件
* RTCDataChannel：使得浏览器之间建立一个高吞吐量、低延时的信道，用于传输任意数据

> https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API

### WebRTC adapter

虽然 WebRTC 规范已经相对健全稳固了，但是并不是所有的浏览器都实现了它所有的功能，有些浏览器需要在一些或者所有的 WebRTC API上添加前缀才能正常使用。

WebRTC 组织在github上提供了一个 WebRTC适配器（WebRTC adapter）来解决在不同浏览器上实现 WebRTC 的兼容性问题。这个适配器是一个JavaScript垫片，它可以让你根据 WebRTC 规范描述的那样去写代码，在所有支持 WebRTC的浏览器中不用去写前缀或者其他兼容性解决方法。

> https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/adapter.js


### 核心的API：navigator.mediaDevices.getUserMedia

网页调用摄像头需要调用 getUserMedia API，MediaDevices.getUserMedia() 会提示用户给予使用媒体输入的许可，媒体输入会产生一个MediaStream，里面包含了请求的媒体类型的轨道。此流可以包含一个视频轨道（来自硬件或者虚拟视频源，比如相机、视频采集设备和屏幕共享服务等等）、一个音频轨道（同样来自硬件或虚拟音频源，比如麦克风、A/D转换器等等），也可能是其它轨道类型。

它返回一个 Promise 对象，成功后会resolve回调一个 MediaStream 对象；若用户拒绝了使用权限，或者需要的媒体源不可用，promise会reject回调一个 PermissionDeniedError 或者 NotFoundError 。(返回的promise对象可能既不会resolve也不会reject，因为用户不是必须选择允许或拒绝。)

通常可以使用 navigator.mediaDevices 来获取 MediaDevices ，例如：

```js
navigator.mediaDevices.getUserMedia(constraints)
  .then(function(stream) {
    // 使用这个stream
  })
  .catch(function(err) {
    // 处理error
  })
```

>https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices/getUserMedia


### 二维码解析库：JSQR

jsQR 是一个纯 JavaScript 二维码解析库，该库读取原始图像或者是摄像头，并将定位，提取和解析其中的任何QR码。

如果要使用jsQR扫描网络摄像头流，则需要ImageData从视频流中提取，然后可以将其传递给jsQR。

jsQR 导出一个方法，该方法接受4个参数，分别是解码的图像数据，宽、高以及可选的对象进一步配置扫描行为。

imageData：格式为 [r0, g0, b0, a0, r1, g1, b1, a1, ...] 的 Uint8ClampedArray（ 8位无符号整型固定数组） 的rgba像素值。

```js
const code = jsQR(imageData, width, height, options);
if (code) {
  console.log('找到二维码！', code);
}
```

> https://github.com/cozmo/jsQR


## 代码实现

### 流程

整个扫码流程如图所示：
页面初始化，先检查浏览器是否支持mediaDevices相关API，浏览器进行调去摄像头，调用失败，就执行失败回调；调用成功，进行捕获视频流，然后进行扫码识别，没有扫瞄到可识别的二维码就继续扫描，扫码成功后绘制扫描成功图案并进行成功回调。

![process](http://127.0.0.1:8080/images/process.png)

## 总结

### 兼容性

* 即使使用了adapter，getUserMedia API在部分浏览器中也存在不支持的
* 低版本浏览器（如 iOS 10.3以下）、Android小众浏览器（如 IQOO自带浏览器）不兼容
* QQ、微信内置浏览器无法调用

![caniuse](http://127.0.0.1:8080/images/caniuse.png)

### 推荐阅读

[1]. Taking still photos with WebRTC
https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Taking_still_photos

[2]. 如何使用JavaScript访问设备前后摄像头
https://juejin.cn/post/6844904184643321870

[3]. Choosing cameras in JavaScript with the mediaDevices API
https://www.twilio.com/blog/2018/04/choosing-cameras-javascript-mediadevices-api.html

