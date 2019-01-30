---
title: FFmpegNotes
date: 2019-01-24 15:47:21
tags: notes
index_img: http://cdn.pfish.xyz/pic/20190126/DGr1LldOTfLW.png
head_img: http://cdn.pfish.xyz/pic/20190126/DGr1LldOTfLW.png
---
一些关于学习FFmpeg的笔记，从未发现处理视频和音频原来可以这么简单！
<!--more-->

### FFmpeg
> FFmpeg是一个开源免费跨平台的视频和音频流方案，属于自由软件，采用LGPL或GPL许可证。它提供了录制、转换以及流化音视频的完整解决方案。

 [FFmpeg官网](http://ffmpeg.org/)

**以前大多都是进行web或者是爬虫方面的开发，对音视频处理应用方面接触不多 **


### FFmpeg命令
> 确保已正确安装ffmpeg并将其文件夹bin目录的路径加入环境变量中。
> 默认输入的原视频文件名为 input.mp4

在命令行输入 `ffmpeg` 正常显示为下图则说明安装成功，可以使用了。

<img src="http://cdn.pfish.xyz/pic/20190126/jlSaVSTy1OWW.png" width="80%">

#### 从原视频中提取出音频 

`ffmpeg -i input.mp4 -vn -acodec aac -ar 44100 -ab 320k audio.aac`

参数说明：

- -vn 无视频
- -acodec 指定输出音频编码格式
- -ar 音频采样率
- -ab 码率

最后为输出音频文件名 audio.aac
 
#### 从原视频中分离视频

 `ffmpeg -i input.mp4 -an -vcodec h264 video.mp4`

 参数说明：
- -an 无音频
- -vcodec 指定输出视频编码格式

最后输出视频文件名video.mp4

#### 将图片和音乐变成视频文件

`ffmpeg -r 1/5 -i %d.jpg -i audio.aac -vcodec h264 -r 24 video.mp4`

参数说明：
- -r 每张图片的停留时间
- %d.jpg 匹配文件夹中的图片
- -vcodec 指定输出视频编码格式
- -r 帧

最后输出视频文件名video.mp4

#### 将彩色视频转换成黑白
 
`ffmpeg -i input.mp4 -vf lutyuv="u=128:v=128" blackwhite.mp4`

参数说明：
- -vf 视频过滤器

最后输出视频文件名blackwhite.mp4

#### 从一段视频中截取出动态图片

`ffmpeg -ss 20 -t 10 -i input.mp4 output.gif`

参数说明：
- -ss 选择从视频第几秒开始
- -t 时长

最后输出动态图片文件名output.gif

#### 生成音频的音频波型图

`ffmpeg -i audio.aac -lavfc showwavepic wave.png`

参数说明：
- -lavfi 音频过滤器
- showwavespic 生成波形图的命令参数

最后输出波形图文件名wave.png

<img src="http://cdn.pfish.xyz/pic/20190126/SPutfAzJXFsd.png?imageslim" width="80%">
