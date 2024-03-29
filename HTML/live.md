| 协议       | 传输层协议 | 应用领域  | 优缺点                                           |
| ---------- | ---------- | --------- | ------------------------------------------------ |
| HTTP-FLV   | TCP        | 直播/点播 | 兼容性好，低延迟，保密性不强                     |
| HLS        | TCP        | 点播      | Apple 支持度高，移动端兼容性好，但延迟较高       |
| RTMP       | TCP        | 直播      | Adobe 支持度高，低延迟，但有累积延迟             |
| RTMFP      | UDP        | 直播      | 带宽消耗低、数据传输速率高，需 Flash Player 支持 |
| RTSP + RTP | TCP + UDP  | IPTV      | 支持组播、效率较高，存在丢包问题，               |

在做监控系统类型的 Web 项目中，一般会采用 RTSP（Real Time Streaming Protocol）协议或者 RTMP（Real-Time Messaging Protocol）协议进行视频流的传输和交互。

- RTSP 是一种标准的网络流媒体传输协议，常用于实时音视频传输。它主要负责流媒体的传输控制，支持 RTP（Real-time Transport Protocol）等多种实时流传输协议。RTSP 对实时性有较高的要求，因此在视频监控、在线会议等领域得到了广泛应用。
- RTMP 是 Adobe 公司开发的实时消息传输协议，主要用于 Flash 播放器与服务器之间的音视频交互。RTMP 在传输效率和处理能力上都有很好的表现，是流媒体直播等领域的常用协议。

如果需要同时支持多种终端设备的视频监控，也可以考虑使用 HLS（HTTP Live Streaming）协议。HLS 是苹果公司提出的流媒体传输协议，它将视频流分割成多个小文件，通过 HTTP 协议进行传输，并结合本地缓存技术实现了低延迟、高稳定性的视频传输。HLS 已成为 iOS 和 macOS 平台上的标准流媒体协议，也被广泛应用于直播、点播等领域。

## **一、背景**

在遍地都是摄像头的今天，往往需要在各种信息化、数字化、可视化 B/S 系统中集成实时视频流播放等功能，海康、大华、华为等厂家摄像头或录像机等设备一般也都遵循监控行业标准，支持国际标准的主流传输协议 RTSP 输出，而 Chrome、Firefox、Edge 等新一代浏览器从 2015 年开始取消了 NPAPI 插件技术支持导致 RTSP 流无法直接原生播放了，这对于绝大部分没有视频处理经验的前、后端工程师来说是一个非常头疼的问题，专业性强，技术门槛高，而对做 B/S 信息化系统集成的公司来说，为了这个模块的功能单独招聘专职研发人员来负责的话，成本高昂不说，还未必做的好。

## **二、现状**

当前主流版本浏览器已经都不支持原生播放 RTSP 流，而且浏览器厂家也明确宣布不考虑支持的情况下，为了在全终端和全平台播放 RTSP 流，一般来说就只能采取在后端先转码再转流给前端播放的方案，这也是号称无插件的技术方案。而对于终端硬件配置较好的场景，也可以采用在后端转流到前端，前端再通过 WASM 程序转码播放的方案，但 IE 并不支持。转码到前端时，即使配置了性能不错的电脑，还受限于 WASM 的固有缺陷，比如多线程支持差、能使用的内存始终受限、只能软解码等，无法充分利用终端电脑的硬件加速能力(GPU)，这就导致同时播放多路或高清 RTSP 流时也会比较吃力，而且大量占用终端电脑的 CPU 和内存资源，其它操作基本无法进行，对音视频格式的兼容能力也很有限。

虽然无插件播放方案能够播放出画面，但是往往延迟很高，基本上都在数秒之久，在一些对延迟敏感的场合客户要求毫秒级延迟，显然无插件技术方案是无法满足的；而且首屏画面显示慢，基本上是数秒级别，这就导致切换播放源时迟迟看不到画面出来，用户体验很差；况且无插件技术方案，需要在后端持续运行高负荷运转的视频转码转流服务，如果摄像头路数多或需要在线播放的终端比较多，服务器的压力就会很大，播放卡顿、花屏、黑屏、断播等现象就会时常出现，很难让客户满意。为了解决这些问题，相关硬件、软件的投入和持续不断的带宽占用往往也让客户难以接受。现在越来越多的客户追求高大上的视频播放效果，采用高清摄像头的越来越多，播放显示器 1080P 已是低配，2K 甚至 4K 大屏正在成为主流之选。这种无插件技术方案，在中高配的屏幕上如果只能播放出慢如蜗牛的画面，想不让客户吐槽实在是太难了。

一个好的视频流网页播放方案，首先要能做到持续稳定播放多路视频，需同时支持 H.264 和 H.265 编码，最核心的还是要做到低延迟、切换画面快，另外就是对当前主流版本的浏览器兼容能力要强，还有就是开发接口丰富并可定制，如果还能做到开源或采用免费开源的主流播放引擎，那就最好不过了，毕竟开源在商业领域的应用越来越多，是个大趋势，从系统集成商的角度来说，开源意味着有更多的自主可控机会来降低整个系统的实施风险。
