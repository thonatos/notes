HTML5 Video & Control

手机版的视频播放，选择了较为激进的HTML5来做，所以要求播放器支持M3U8格式视频，找了一堆播放器测试，发现功能大同小异，有一个感觉很不错的，但是价格有点伤人，况且我们需要的功能其实很简单，所以，自己写了。

#### HTML5 Player

* VideoJS
* Flowplayer
* Html5Media
* MediaElements

vjs直接忽略了，不知道他们怎么弄的，直接没法支持hls的播放，直接忽略吧~

#### Do something

	var h5m = document.getElementById('html5-video');
	
	// Get Property
	
	var _h5mSrc = h5m.src;
	var _h5mDuration = h5m.duration.toFixed(1);
	var _h5mCurrentTime = h5m.currentTime.toFixed(1);

	// Set Property
	
	h5m.currentTime = _h5mCurrentTime;
	h5m.src = newSrcString;
	
	// Bind Event
	
	h5m.addEventListener("onended", function(){
		console.log("end");
		// and some thing
	});
	
	h5m.addEventListener("oncanplay", function(){
		console.log("canplay");
		// and some thing
	});
	
	h5m.addEventListener("onplaying", function(){
		console.log("playing");
		// and some thing
	});

有了上述的方法，然后配合一些前端技巧，可以随意定制了~

** 属性和方法参考后面，摘自w3cschool. **

#### HTML5 Video Intro

使用方法：

	<video width="320" height="240" controls>
		<source src="movie.mp4" type="video/mp4">
		<source src="movie.ogg" type="video/ogg">
		您的浏览器不支持Video标签。
	</video>
	
支持格式： MP4, WebM, 和 Ogg

* MP4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
* WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件
* Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件

支持浏览器：

<table width="100%">
    <tbody>
    <tr>
        <th width="25%">浏览器</th>
        <th width="25%">MP4</th>
        <th width="25%">WebM</th>
        <th width="25%">Ogg</th>
    </tr>
    <tr>
        <td>Internet Explorer 9+</td>
        <td>YES</td>
        <td>NO</td>
        <td>NO</td>
    </tr>
    <tr>
        <td>Chrome 6+</td>
        <td>YES</td>
        <td>YES</td>
        <td>YES</td>
    </tr>
    <tr>
        <td>Firefox 3.6+</td>
        <td>NO</td>
        <td>YES</td>
        <td>YES</td>
    </tr>
    <tr>
        <td>Safari 5+</td>
        <td>YES</td>
        <td>NO</td>
        <td>NO</td>
    </tr>
    <tr>
        <td>Opera 10.6+</td>
        <td>NO</td>
        <td>YES</td>
        <td>YES</td>
    </tr>
    </tbody>
</table>

#### HTML5 Video Property

可选属性：

<table width="100%">
    <tbody>
    <tr>
        <th align="left" width="20%">属性</th>
        <th align="left" width="20%">值</th>
        <th align="left" width="50%">描述</th>
    </tr>
    <tr>
        <td>autoplay</td>
        <td>autoplay</td>
        <td>如果出现该属性，则视频在就绪后马上播放。</td>
    </tr>
    <tr>
        <td>controls</td>
        <td>controls</td>
        <td>如果出现该属性，则向用户显示控件，比如播放按钮。</td>
    </tr>
    <tr>
        <td>height</td>
        <td><i>pixels</i></td>
        <td>设置视频播放器的高度。</td>
    </tr>
    <tr>
        <td>loop</td>
        <td>loop</td>
        <td>如果出现该属性，则当媒介文件完成播放后再次开始播放。</td>
    </tr>
    <tr>
        <td>muted</td>
        <td>muted</td>
        <td>如果出现该属性，视频的音频输出为静音。</td>
    </tr>
    <tr>
        <td valign="top">poster</td>
        <td valign="top"><em>URL</em></td>
        <td valign="top">规定视频正在下载时显示的图像，直到用户点击播放按钮。</td>
    </tr>
    <tr>
        <td>preload</td>
        <td>auto<br> metadata<br> none</td>
        <td>如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。</td>
    </tr>
    <tr>
        <td>src</td>
        <td><i>URL</i></td>
        <td>要播放的视频的 URL。</td>
    </tr>
    <tr>
        <td>width</td>
        <td><i>pixels</i></td>
        <td>设置视频播放器的宽度。</td>
    </tr>
    </tbody>
</table>

#### HTML5 Video Event

多媒体事件(Media Events)

通过视频（videos），图像（images）或者音频（audio） 触发该事件，多应用于HTML媒体元素比如:

	<audio>, <embed>, <img>, <object>, <video>

<table width="100%">
    <tbody>
    <tr>
        <th style="width:28%;">属性</th>
        <th style="width:8%;">值</th>
        <th>描述</th>
    </tr>
    <tr>
        <td>onabort</td>
        <td><i>script</i></td>
        <td>当发生中止事件时运行脚本</td>
    </tr>
    <tr>
        <td>oncanplay</td>
        <td><i>script</i></td>
        <td>当媒介能够开始播放但可能因缓冲而需要停止时运行脚本</td>
    </tr>
    <tr>
        <td>oncanplaythrough</td>
        <td><i>script</i></td>
        <td>当媒介能够无需因缓冲而停止即可播放至结尾时运行脚本</td>
    </tr>
    <tr>
        <td>ondurationchange</td>
        <td><i>script</i></td>
        <td>当媒介长度改变时运行脚本</td>
    </tr>
    <tr>
        <td>onemptied</td>
        <td><i>script</i></td>
        <td>当媒介资源元素突然为空时（网络错误、加载错误等）运行脚本</td>
    </tr>
    <tr>
        <td>onended</td>
        <td><i>script</i></td>
        <td>当媒介已抵达结尾时运行脚本</td>
    </tr>
    <tr>
        <td>onerror</td>
        <td><i>script</i></td>
        <td>当在元素加载期间发生错误时运行脚本</td>
    </tr>
    <tr>
        <td>onloadeddata</td>
        <td><i>script</i></td>
        <td>当加载媒介数据时运行脚本</td>
    </tr>
    <tr>
        <td>onloadedmetadata</td>
        <td><i>script</i></td>
        <td>当媒介元素的持续时间以及其他媒介数据已加载时运行脚本</td>
    </tr>
    <tr>
        <td>onloadstart</td>
        <td><i>script</i></td>
        <td>当浏览器开始加载媒介数据时运行脚本</td>
    </tr>
    <tr>
        <td>onpause</td>
        <td><i>script</i></td>
        <td>当媒介数据暂停时运行脚本</td>
    </tr>
    <tr>
        <td>onplay</td>
        <td><i>script</i></td>
        <td>当媒介数据将要开始播放时运行脚本</td>
    </tr>
    <tr>
        <td>onplaying</td>
        <td><i>script</i></td>
        <td>当媒介数据已开始播放时运行脚本</td>
    </tr>
    <tr>
        <td>onprogress</td>
        <td><i>script</i></td>
        <td>当浏览器正在取媒介数据时运行脚本</td>
    </tr>
    <tr>
        <td>onratechange</td>
        <td><i>script</i></td>
        <td>当媒介数据的播放速率改变时运行脚本</td>
    </tr>
    <tr>
        <td>onreadystatechange</td>
        <td><i>script</i></td>
        <td>当就绪状态（ready-state）改变时运行脚本</td>
    </tr>
    <tr>
        <td>onseeked</td>
        <td><i>script</i></td>
        <td>当媒介元素的定位属性 [1] 不再为真且定位已结束时运行脚本</td>
    </tr>
    <tr>
        <td>onseeking</td>
        <td><i>script</i></td>
        <td>当媒介元素的定位属性为真且定位已开始时运行脚本</td>
    </tr>
    <tr>
        <td>onstalled</td>
        <td><i>script</i></td>
        <td>当取回媒介数据过程中（延迟）存在错误时运行脚本</td>
    </tr>
    <tr>
        <td>onsuspend</td>
        <td><i>script</i></td>
        <td>当浏览器已在取媒介数据但在取回整个媒介文件之前停止时运行脚本</td>
    </tr>
    <tr>
        <td>ontimeupdate</td>
        <td><i>script</i></td>
        <td>当媒介改变其播放位置时运行脚本</td>
    </tr>
    <tr>
        <td>onvolumechange</td>
        <td><i>script</i></td>
        <td>当媒介改变音量亦或当音量被设置为静音时运行脚本</td>
    </tr>
    <tr>
        <td>onwaiting</td>
        <td><i>script</i></td>
        <td>当媒介已停止播放但打算继续播放时运行脚本</td>
    </tr>
    </tbody>
</table>

