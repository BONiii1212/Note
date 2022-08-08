## WebRTC 

#### 简介

一项实时通讯技术，允许网络应用或站点，在不借助中间媒介的情况下，建立浏览器之间点对点（Peer-to-Peer）的连接，实现视频流、音频流和其他任意数据的传输。

#### WebRTC点对点通行原理

- 怎么知道彼此的存在

  需要信令服务器，来交换元数据如媒体信息(sdp)、网络数据等等信息。

- 彼此音频编码能力如何沟通

  通过信令服务器交换彼此的SDP信息，来了解对方支持的媒体格式

- 音视频数据如何传输

  信令服务器交换彼此的IceCandidate来了解网络情况，通过NAT建立网络链接，ICE来解决NAT的问题，ICE是整合了STUN和TURN两种协议的框架

简而言之就是通过 WebRTC 提供的 API 获取各端的媒体信息 SDP 以及 网络信息candidate ，并通过信令服务器交换，进而建立了两端的连接通道完成实时视频语音通话。

#### 类型介绍

其中描述信息为RTCSessionDescription格式

```js
{
type: 'answer', 
sdp: 'v=0\r\no=- 212087045958721586 2 IN IP4 127.0.0.1\r\ns=…oDf78\r\na=ssrc:4183397335cname:7WqAa9eRt1ooDf78\r\n'
}
```

其中SDP为peer-to-peer连接的标准，SDP包含音视频的：编解码，源地址和事件信息

```
v=0
o=alice 2890844526 2890844526 IN IP4 host.anywhere.com
s=
c=IN IP4 host.anywhere.com
t=0 0
m=audio 49170 RTP/AVP 0
a=rtpmap:0 PCMU/8000
m=video 51372 RTP/AVP 31
a=rtpmap:31 H261/90000
m=video 53000 RTP/AVP 32
a=rtpmap:32 MPV/90000
```

协商信息RTCIceCandidate

```js
{
  address: "192.168.1.100"
  candidate: "candidate:3013953624 1 udp 2122260223 192.168.1.100 56512 typ host generation 0 ufrag BcCs network-id 1 network-cost 10"
  component: "rtp"
  foundation: "3013953624"
  port: 56512
  priority: 2122260223
  protocol: "udp"
  relatedAddress: null
  relatedPort: null
  sdpMLineIndex: 0
  sdpMid: "0"
  tcpType: null
  type: "host"
  usernameFragment: null
}
```

#### STUN

起因：NAT技术虽然在一定程度上解决了IPv4地址缺陷的问题，但其由于保护网络内部的主机，会过滤掉一些外网主动发送到内网的报文，使得阻断了P2P网络的主动访问。

解决方法：反向链接技术、应用层网关ALG技术、打洞技术、中间件技术。

STUN是一种“打洞”技术，NAT会话穿越应用程序STUN，STUN会检测网络中是否存在NAT设备，从而获取两个通信端点经过NAT设备分配的IP地址和端口号，然后在两个通信端点之间建立一条可穿越NAT的P2P链接

#### TURN

用于解决NAT类型是对称型的话，STUN就无法成功打洞，TURN是STUN的一个拓展协议，在其基础上添加了Replay中继功能，用于解决对称NAT无法穿越问题

#### WebRTC创建连接步骤

MDN地址：https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API/Connectivity

1. 呼叫者通过 `navigator.mediaDevices.getUserMedia()`捕捉本地媒体。
2. 呼叫者创建一个`RTCPeerConnection` 并调用 `RTCPeerConnection.addTrack()`
3. 呼叫者调用 `RTCPeerConnection.createOffer()`来创建一个提议 (offer).
4. 呼叫者调用`RTCPeerConnection.setLocalDescription()`将提议 (offer) 设置为本地描述 (即，连接的本地描述).
5. setLocalDescription() 之后，呼叫者请求 STUN 服务创建 ice 候选 (ice candidates)
6. 呼叫者通过信令服务器将提议 (offer) 传递至 本次呼叫的预期的接受者。
7. 接受者收到了提议 (offer) 并调用 `RTCPeerConnection.setRemoteDescription()` 将其记录为远程描述 (也就是连接的另一端的描述).
8. 接受者做一些可能需要的步骤结束本次呼叫：捕获本地媒体，然后通过`RTCPeerConnection.addTrack()`添加到连接中。
9. 接受者通过`RTCPeerConnection.createAnswer()` 创建一个应答。
10. 接受者调用 `RTCPeerConnection.setLocalDescription()` 将应答 (answer) 设置为本地描述。此时，接受者已经获知连接双方的配置了。
11. 接受者通过信令服务器将应答传递到呼叫者。
12. 呼叫者接受到应答。
13. 呼叫者调用 `RTCPeerConnection.setRemoteDescription()`将应答设定为远程描述。如此，呼叫者已经获知连接双方的配置了。

