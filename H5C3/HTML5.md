### 1、音频和视频标签

在了解音频和视频内容之前先了解以下知识点：



![32](..\images\32.png)



![33](..\images\33.png)

![34](..\images\34.png)

![35](..\images\35.png)

````
5. 格式转化
用 FFmpeg 制作MP4 视频
ffmpeg -i test.mp4 -c:v libx264 -s 1280x720 -b:v 1500k -profile:v high -level 3.1 -c:a aac -ac 2 -b:a 160k -movflags faststart OUTPUT.mp4

用 FFmpeg 制作 WebM 视频
ffmpeg -i test.mp4 -c:v libvpx -s 1280x720 -b:v 1500k -c:a libvorbis -ac 2 -b:a 160k OUTPUT.webm

FFmpeg 制作 Ogg 视频
ffmpeg -i test.mp4 -c:v libtheora -s 1280x720 -b:v 1500k -c:a libvorbis -ac 2 -b:a 160k OUTPUT.ogv

FFmpeg 制作Mp3音频
ffmpeg -i test.mp3 -c:a libmp3lame -ac 2 -b:a 160k OUTPUT.mp3

FFmpeg 制作Ogg音频
ffmpeg -i test.mp3 -c:a libvorbis -ac 2 -b:a 160k OUTPUT.ogg
ffmpeg -i IDon'tWannaLiveForever.mp3 -c:a libvorbis -ac 2 -b:a 160k IDon'tWannaLiveForever.ogg


FFmpeg 制作ACC音频
ffmpeg -i test.mp3 -c:a aac -ac 2 -b:a 160k OUTPUT.aac
````

因为对于音频和视频文件各个浏览器之间存在兼容问题，所以我们在开发中需要将视频或者音频文件转为各个版本的文件，可以用ffmpeg工具结合cmd命令进行格式转化！

***ffmpeg的使用方法：***

使用的基本条件是：.exe后缀应用程序启动+需要转换格式的视频文件在同一个文件目录下！但是这样有点麻烦所以可以配置环境变量！把ffmpeg文件的bin目录添加到 系统环境变量的 Path下！然后直接在视频文件目录下打开cmd输入上面的第五步里面的命令转格式！

给2个参考链接：

<https://www.cnblogs.com/tianxiaxuange/p/9986948.html>

<https://blog.csdn.net/WuLex/article/details/101513018>

