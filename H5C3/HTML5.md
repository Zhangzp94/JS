### 1、音频和视频标签

##### 01 前戏

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

ffmpeg下载地址：https://ffmpeg.zeranoe.com/builds/

配置：右击此电脑——>属性——>高级系统设置——>环境变量。在系统变量的path变量里添加解压的路径。

如：\ffmpeg-4.0-win64-static\ffmpeg-4.0-win64-static\bin

##### 02 video标签和audio标签

```js
<!--
浏览器会根据type格式优先判断浏览器是否支持当前的格式，然后逐层匹配,一般视频准备下面三种格式。

-->
<video controls width="300" >
    <source src="./images/one.mp4" type="video/mp4">
    <source src="./images/outone.webm" type="video/webm">
    <source src="./images/outone.ogv" type="video/ogg">
    当前浏览器不支持veideo自动播放，请点击下载视频：<a href="./images/one.mp4">下载视频</a>
</video>
<audio controls>
    <source src="./xx.mp3" type="audio/mpeg">
    <source src="./xx.aac" type="audio/aac" codecs="aac">
    <source src="./xx.ogg" type="audio/ogg" codecs="vorbis">
    当前浏览器不支持veideo自动播放，请点击下载音频：<a href="./images/one.mp3">下载音频</a>
</audio>
```

**video标签属性**

````js
width 
height
poster="../logo.png" 一个封面的url，就是视频播放前展示给用户看的封面

src 要嵌入的视频和音频路径
controls 显示和隐藏视频和音频的控制界面
autoplay 媒体是否自动播放
loop 媒体是否循环播放
muted 是否静音
preload 该属性是告诉浏览器作者认为达到最佳的用户体验的方式是什么
		none 提示作者认为用户不需要查看该视频，服务器也想要最小化流量，换句话说就是提醒浏览器该视频不需要缓存；等用户点击播放的时候再去获取视频！
        metadata 提示尽管用户不需要查看该视频，不过抓取元数据（比如：长度）还是很合理的；在用户不点击播放的时候，先获取一些播放界面海报的信息！（一般使用这个默认值）
        auto 用户需要这个视频优先加载，换句话说如果需要的话可以下载整个视频，即使用户不一定会用它
        空字符串：也就是代auto值
        
````

##### 03 音视频js相关属性

````js
var video = document.querySelector("video")
video.duration 媒体总时间（只读）
video.currentTime 开始播放到现在所用的时间（可读写）；配合计时器使用可以输出对应的值

video.muted 是否静音，布尔类型，可读写 video.mute=true[false]
video.volume 音量相对值（可读写）video.volume = 0.0-1.0 之间，
这里注意muted和volume的坑！
在js中设置muted的值的时候，需要把volume同时设置，比如video.muted=true 要加上 video.volume=0

video.paused 媒体是否暂停（只读） 
video.ended 媒体是否播放完毕（只读） 
video.error 媒体是否出错（只读） 
video.currentSrc 以字符串的形式返回媒体地址（只读） 

上面的属性是音视频都有的属性！下面是视频多的部分属性
video.poster="../xx.png" 设置视频的海报图像的（可读写）
video.width 
video.height
video.videoWidth  video.videoHeight 视频的分辨率（只读） 
````

##### 04 音视频js相关函数

````
var video = document.querySelector("video")
video.play()  媒体播放

video.pause() 媒体暂停
   setTimeout(function () {
       video.pause()
   },5000)
   
video.load()  重新加载
应用场景:
当通过js去更换video的src的时候，这时候需要重新加载视频才会生效！
source.src = '../....'
video.load()
````



##### 05 音视频的js事件

<img src="..\images\36.png" alt="36" style="zoom:100%;" />

![37](..\images\37.png)



***事件 canplay***

当音视频加载完毕可以播放的时候后触发 canplay事件

场景：点击下一首歌曲的时候，快速地点击会触发一些错误，所以可以设置一个锁，在canplay事件触发后再播放下一首！

**事件play**

play是播放的时候触发，canplay是加载到可以播放的时候触发！play是后面触发！

***事件 timeupdate***

只要一播放，就会触发这个事件，

````js
audio.addEventListener('timeupdate',(e)=>{ e.target.currentTime })
````



***事件ended***



### 2、表单验证

##### 01 invalid事件

这个事件是当input需要属性为required （需要填写内容）的时候，如果内容为空，点击submit的时候就会触发这个事件！

触发事件满足的条件：1. Input内验证失败 2.点击了submit

表单验证有如下的属性，属性是在input.validity这个对象上

```js
<form action=""> //主要要加form
    <input type="text" required pattern="\d{1,5}" id="text"/>
    <input type="submit" value="提交"/>
</form>
<script>
    var Ele = document.querySelector("#text")
    Ele.addEventListener("invalid",function () {
        console.log(this.validity)
    })
</script>
```



![40](..\images\40.png)![39](..\images\39.png)

##### 02 setCustomValidity() 

当验证失败的时候，input会自动提示错误信息，这个API 可以修改错误信息的内容！

![41](..\images\41.png)

```js
var Ele = document.querySelector("#text")
Ele.addEventListener("invalid",function () {
    console.log(this.validity)
    this.setCustomValidity("请输入正确的格式信息！")
})
```



### 3、svg标签

参考链接

https://www.cnblogs.com/daisygogogo/p/11044353.html

random

exchange  

ban