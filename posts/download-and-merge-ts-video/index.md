# TS 格式视频的下载与合并


TS的全称则是Transport Stream，也就是传输流，是现在很多视频站点使用的视频播放技术，而不是传统的直接播放 MP4/FLV 文件，其特点是把视频分割成很多个 `.ts` 格式的切片，每个文件都很小巧，大概几秒几百K，然后通过 `.m3u8`  格式的切片列表将文件连接起来进行播放。

但是这种技术在下载视频的时候就有点麻烦了，一般的视频嗅探工具只能抓取到一个一个的切片文件，有时候1分钟的视频会被分割成将近20个，还得再用工具拼接起来，有些繁琐了。

<!--more-->

![](//static.u1s3.com/images/54526ac87a585d021d280c078c6996f5.png)

在搜索下载方式的过程中发现可以通过 FFmpeg 程序自动下载 `.m3u8` 中的切片并合并成 `.mp4` 文件的方式，尝试了一下，效果不错，使用起来也很简单，一句话就行。

```bash
$ ffmpeg -i https://domain.com/playlist.m3u8 -c copy -bsf:a aac_adtstoasc output.mp4
```

但是每次都要从头输入命令就有点烦人，这个时候就可以编写一个简单的 Python 脚本来调用 FFmpeg 自动下载 `.m3u8` 中的 `.ts` 切片并自动合并成 `.mp4` 文件。

## 准备开始

既然要用到 Python 脚本，那首先得安装一下它，Manjaro 中自带了 Python3，其他系统没有的话安装一下就行。

然后还要用到 FFmpeg 来下载合并切片文件，Manjaro 中也是默认就装好了，真好，不用自己安装，其他 Linux 或 Mac 可以通过常规方法安装，Windows 可以直接去[官网](https://www.ffmpeg.org/)下载 `.exe` 文件，然后添加到系统环境变量之后再使用 PowerShell 来调用。

## 开始下载

### 获取切片列表

第一步当然是要拿到 `.m3u8` 切片列表，这个可以通过 Chrome 的开发者工具中的 Network 标签取得，如果找不到的话刷新一下页面重新加载视频。

![](//static.u1s3.com/images/c5d349c8df8e4f6eafad571bc420e29b.png)

### 编写脚本

这里提供两种方式：

第一种方式：单个视频下载，运行脚本后提示用户输入 `.m3u8` 文件地址及合并后的文件名。

```python
# 第一种方式：单个视频下载，提示用户输入 .m3u8 文件地址及合并后的文件名
import os
m3u8Url = input('url:')
outputName = input('outputName:')
print(os.system('ffmpeg -i ' + m3u8Url + ' -c copy -bsf:a aac_adtstoasc ' + outputName + .mp4')
```

第二种方式：多个视频下载，将 `.m3u8` 文件地址及合并后的文件名写到数组里。

```python
import os
items = [
  ['outputName1', 'm3u8Url1'],
  ['outputName2', 'm3u8Url2'],
]
for item in items:
  print(os.system('ffmpeg -i ' + item[1] + ' -c copy -bsf:a aac_adtstoasc ' + item[0] + .mp4'))
```

### 运行脚本

选择上面任意一种方式，保存到 *download.py* 文件中，然后运行 `python download.py`，之后剩下的就是等待。

## 参考文档

[FFmpeg](https://www.ffmpeg.org/)

[Python](https://www.python.org/)

[下载网页视频并自动合成视频](https://blog.csdn.net/realDonaldTrump/article/details/82697757)
