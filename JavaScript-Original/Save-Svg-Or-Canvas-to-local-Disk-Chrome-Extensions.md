Chrome Extensions - Save Svg/Canvas to local Disk.

使用js动态生成的svg活着canvas图像（保存为png）要保存到本地，直接在浏览器下载到本地，需要做一些处理。

#### Convert to base64

需要进行转换的原因是直接下载貌似不太方便，或者是我没找到相关的接口。

Svg:

	var svg = $('#qrcode-image svg').get(0);
	var serializer = new XMLSerializer();
	var serializerStr = serializer.serializeToString(svg);
	var svgBase64 = "data:image/svg+xml;base64,\n"+util.Base64.encode(serializerStr);
	
Canvas:

	canvas一般情况本身就是base64格式。
	
#### Download

	# manifest.json
	
	{
		...
		"permissions": [
			"tabs",
			"downloads"
		],
		...
	}

	# script
	
	chrome.downloads.download({url: svgBase64, filename: 'qrcode-image.svg'}, function (downloadId) {
		console.log("download begin, the downId is:" + downloadId);
	});

#### Reference

http://stackoverflow.com/questions/246801/how-can-you-encode-a-string-to-base64-in-javascript