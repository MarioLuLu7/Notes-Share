# vue-video-player HLS 加密

最近有一个项目，需要实现实时监控视频流播放，PC 和 mobile 端都要有此功能，由于 mobile 端我是使用 vue 写的，所以选用 vue-video-player。
这之前其实有一个插曲，本来我是打算使用 flv.js 两端同时实现的，但是移动端浏览器都不支持 flv 格式的视频流（flash 的原因）。写这篇文章的时候，我查到有人用 canvas 渲染的方法使得可以在移动端播放 flv，插件是 FlvPlayer.js，各位可以尝试一下。

## HLS

这篇文章讲的是 HLS，全称 HTTP Live Streaming，是一个由苹果公司提出的基于 HTTP 的流媒体网络传输协议。HLS 提供的是一个 m3u8 的地址， 苹果的 safari 可以直接打开这个地址，但是 Android 浏览器需要用一个 video 标签包一下。

```
<video width="600" height="300"
  src="xxxxxxx.m3u8"
  type="application/vnd.apple.mpegurl">
</video>
```

浏览器请求到了 HLS 协议并分析出协议里的视频文件

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-ALLOW-CACHE:YES
#EXT-X-MEDIA-SEQUENCE:24057
#EXT-X-TARGETDURATION:9
#EXTINF:7.959, no desc
nvr_hik_sx02-24057.ts
#EXTINF:7.960, no desc
nvr_hik_sx02-24058.ts
#EXTINF:7.959, no desc
nvr_hik_sx02-24059.ts
#EXTINF:4.159, no desc
nvr_hik_sx02-24060.ts
#EXT-X-DISCONTINUITY
#EXTINF:7.959, no desc
nvr_hik_sx02-24061.ts
```

每个切片格式是 ts，也就是告诉浏览器，播放这些 ts 格式的视频。但是这些操作都是浏览器帮我们实现的，当我想要往每个切片后添加加密字段就变得很难，所以我选择使用 vue-video-player（video.js）来实现。

## 加密

首先，需要让 vue-video-player 能请求到 HLS 协议，然后在请求视频之前加上加密字段。

```
<video-player
  ref="videoPlayer"
  :options="playerOptions"
  :playsinline="true"
  customEventName="customstatechangedeventname"
  @ready="playerReadied">
</video-player>

import 'video.js/dist/video-js.css'
import 'vue-video-player/src/custom-theme.css'
import { videoPlayer } from 'vue-video-player'
import 'videojs-contrib-hls'

...

data() {
  return {
    playerOptions: {
      flash: { hls: { withCredentials: false } },
      // 这里对于html5的配置很重要
      html5: {
        hls: { withCredentials: false, overrideNative: true },
        nativeAudioTracks: false,
        nativeVideoTracks: false
      },
      muted: true,
      autoplay: true,
      language: 'zh-CN',
      notSupportedMessage: '此视频暂无法播放，请稍后再试',
      aspectRatio: '16:9',
      fluid: true,
      sources: [{
        type: 'application/x-mpegURL',
        src: '视频资源视频资源',
        overrideNative: true
      }],
      controlBar: {
        timeDivider: false,
        durationDisplay: false
      }
    }
  }
}

...

methods: {
  playerReadied (player) { // 拿到播放器实例
      player.tech_.hls.xhr.beforeRequest = (options) => {
        options.uri += '加密字段加密字段...'
        return options
      }
    }
}

```
