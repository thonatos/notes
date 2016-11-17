# # gyro hack in iframe

> If you use the gyro plugin in an iframe, it can only work if the domain of the main window is the same domain as the iframe. This is a (security) limitation of the browser.

## #solution

get gyro information in the main window & post it to the child iframe.

- iframe
- postMessage
- JSON

在主窗体获取传感器信息，并使用postMessage传递信息到iframe子页面，测试中发现postMessage需要的信息为字符串，所有先做一次JSON.stringify，在子页面做一次JSON.parse即可。

## #code

```
//main window

if (window.DeviceOrientationEvent) {  			

	var iframe_embed_hack = {
		enabled: true
	};				

 	function onUpdate(){
 		document.getElementById('child').contentWindow.postMessage(JSON.stringify(iframe_embed_hack),"*");
 	}

 	function onScreenOrientationChangeEvent(){
 		iframe_embed_hack.screenOrientation = window.orientation || 0;
 	}

 	function onDeviceOrientationChangeEvent(event){
 		iframe_embed_hack.deviceOrientation = event;
 		onUpdate();
 	}

	window.addEventListener('orientationchange', onScreenOrientationChangeEvent, false);
	window.addEventListener('deviceorientation', onDeviceOrientationChangeEvent, false);

} 
```

```
//the child iframe
function messageHandler(data){					
	window.iframe_embed_hack = data;
}
			
window.addEventListener("message", function(ev) { 		            
   messageHandler(JSON.parse(ev.data));		                   
}, false ); 
```