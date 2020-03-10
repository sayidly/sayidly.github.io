---
title: "下载流媒体视频"
published: true
---

有时候我们需要抓取流媒体视频来放到本地播放的需求。

对于许多媒体，[you-get][you-get] 或者是 [youtube-dl][youtube-dl] 都可以很好的满足需求了。

但是对于 [流媒体][stream-video](`.m3u8`)，这两个工具却显得有点束手无策。

举个例子 [Daily Motion][daily-motion] 上有很多值得珍藏的优秀的视频。但是他们都是以流媒体格式 `.m3u8` 来播放的，当你想存储在电脑里以后进行播放的时候，就会遇到麻烦。

下面以 daily motion 上 [BBC Our Secret Universe The Hidden Life of the Cell][video-example] 这个片子为例子来讲解如何保存流媒体文件。

声明：本教程适用于学习目的的视频抓取，严禁用作其他非法的用途。

## 准备工作

需要准备以下的工具（mac 上为例）

- [ffmpeg][ffmpeg]

终端下载 ffmpeg
```
brew install ffmpeg
```

## 发现流媒体文件 `.m3u8`

这一个步骤有一点难，需要在浏览器的开发模式中查找。以 Chrome 为例：

1. 打开页面 [BBC Our Secret Universe The Hidden Life of the Cell][video-example]；
2. 在流媒体视频的页面右键点击 -> 审查元素 -> 网络；
3. 在搜索框中搜索 `.m3u8`，点击对应的文件之后；
4. 在右侧点击 Headers，找到 `Request URL`，复制该地址。
    ```
    https://proxy-28.sv4.dailymotion.com/sec(vH_amhAWVjdOjt9rQ-usyG-Gmc5smGQJOp-v33gc11ujVrsQmJqt-hBnszgfId9QYBfxXacsGI7qgvN0kql9nZFDHRydeMpJclwqwE5_ELk)/video/787/673/380376787_mp4_h264_aac_hd.m3u8
    ```

![Steam Video Network]({{ site.baseurl }}/assets/images/stream-video-m3u8.jpg)


## 观看流媒体

可以用本地的播放器来观看流媒体 [IINA](https://iina.io/), 或是 [VLC media player](https://www.videolan.org/index.zh.html) 都是很棒的工具。


## 下载流媒体

这一步就需要用到前面准备工作中下载的 [ffmpeg][ffmpeg]，下载完成之后运行命令

```bash
$ echo "Enter m3u8 link:";read link;echo "Enter output filename:";read filename;ffmpeg -i "$link" -bsf:a aac_adtstoasc -vcodec copy -c copy -crf 50 $filename.mp4
```

输入对应的刚刚复制的 **Request URL** 和 **想要导出的文件名**。

等待下载完毕即可。

关于 ffmpeg 的使用可以参考阮一峰的教程 [FFmpeg 视频处理入门教程](https://www.ruanyifeng.com/blog/2020/01/ffmpeg.html)

## 参考

- [FFmpeg 视频处理入门教程](https://www.ruanyifeng.com/blog/2020/01/ffmpeg.html) - 阮一峰
- [m3u8 stream to mp4 using ffmpeg](https://gist.github.com/tzmartin/fb1f4a8e95ef5fb79596bd4719671b5d)


[you-get]: https://github.com/soimort/you-get
[youtube-dl]: https://github.com/ytdl-org/youtube-dl
[stream-video]: https://en.wikipedia.org/wiki/Streaming_media
[daily-motion]: https://www.dailymotion.com/
[ffmpeg]: https://www.ffmpeg.org/
[video-example]: https://www.dailymotion.com/video/x6agslv
