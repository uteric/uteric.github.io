---
title: 使用youtube-dl命令下载youtube视频与音频
date: 2019-12-28 18:08:35
updated: 2019-12-28 18:08:35
tags: YouTube
categories: 编程语言
---


## 1、安装youtube-dl

```
sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
hash -r
```
现在，你可以使用如下命令来将youtube-dl更新到最新版本：

```
sudo youtube-dl -U # 版本更新
youtube-dl --version # 检查版本
```
如果你之前使用apt命令安装youtube-dl但无法正常使用，请使用如下命令卸载并重新安装：

```
sudo apt-get remove -y youtube-dl
```

## 2、获取视频与音频信息

以Jingle Bells这首儿歌为例，

```
youtube-dl -F https://www.youtube.com/watch?v=eQ34DSTjsLQ
```

返回如下信息：

```
[youtube] eQ34DSTjsLQ: Downloading webpage
[youtube] eQ34DSTjsLQ: Downloading video info webpage
[info] Available formats for eQ34DSTjsLQ:
format code  extension  resolution note
249          webm       audio only tiny   83k , opus @ 50k (48000Hz), 1.48MiB
250          webm       audio only tiny  104k , opus @ 70k (48000Hz), 1.87MiB
140          m4a        audio only tiny  130k , m4a_dash container, mp4a.40.2@128k (44100Hz), 2.93MiB
251          webm       audio only tiny  178k , opus @160k (48000Hz), 3.46MiB
394          mp4        256x144    144p   80k , av01.0.00M.08, 24fps, video only, 1.64MiB
278          webm       256x144    144p   97k , webm container, vp9, 24fps, video only, 2.08MiB
160          mp4        256x144    144p  109k , avc1.4d400c, 24fps, video only, 2.08MiB
395          mp4        426x240    240p  168k , av01.0.00M.08, 24fps, video only, 3.19MiB
242          webm       426x240    240p  222k , vp9, 24fps, video only, 4.24MiB
133          mp4        426x240    240p  292k , avc1.4d4015, 24fps, video only, 4.61MiB
396          mp4        640x360    360p  299k , av01.0.01M.08, 24fps, video only, 5.49MiB
243          webm       640x360    360p  408k , vp9, 24fps, video only, 7.19MiB
397          mp4        854x480    480p  515k , av01.0.04M.08, 24fps, video only, 9.39MiB
134          mp4        640x360    360p  528k , avc1.4d401e, 24fps, video only, 8.33MiB
244          webm       854x480    480p  751k , vp9, 24fps, video only, 10.84MiB
135          mp4        854x480    480p  800k , avc1.4d401e, 24fps, video only, 11.53MiB
398          mp4        1280x720   720p 1044k , av01.0.05M.08, 24fps, video only, 19.72MiB
136          mp4        1280x720   720p 1098k , avc1.4d401f, 24fps, video only, 16.32MiB
247          webm       1280x720   720p 1297k , vp9, 24fps, video only, 18.38MiB
18           mp4        640x360    360p  667k , avc1.42001E, mp4a.40.2@ 96k (44100Hz), 15.09MiB (best)

```

每个视频或音频前都有一个编号，如140代表m4a音频，18代表mp4视频等，现在，可以开始下载了。

## 3、下载视频或音频

```
youtube-dl -f 140 https://www.youtube.com/watch?v=eQ34DSTjsLQ # 下载m4a音频
youtube-dl -f 18 https://www.youtube.com/watch?v=eQ34DSTjsLQ # 下载mp4视频
```

OK，all done!
