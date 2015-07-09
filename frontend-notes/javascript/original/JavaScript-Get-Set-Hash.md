JavaScript Hash参数的获取与设置

做微信海报页面需要获取用户名信息，用来显示邀请函，但是微信JS-SDK对6.02版本以下的支持并不好，在Android平台无法设置自定义信息，所以需要人为的处理以下，原先的解决方案是通过后台打出数据，然后来判断，现在换成Hash的方式来判断。

#### Hash参数获取

1.通过正则的方式获取参数

    function getQuery(query) {
        query = query.replace(/[\[]/, "\\\[").replace(/[\]]/, "\\\]");
        var expr = "[\\?&]" + query + "=([^&#]*)";
        var regex = new RegExp(expr);
        var results = regex.exec(window.location.href);
        if (results !== null) {
            return results[1];
        } else {
            return false;
        }
    }
 2.通过常规手段获取
 
     function getQueryUrl() {
        var name, value;
        var str = location.href;
        var num = str.indexOf("?");
        str = str.substr(num + 1);
    
        var arr = str.split("&");
        for (var i = 0; i < arr.length; i++) {
            num = arr[i].indexOf("=");
            if (num > 0) {
                name = arr[i].substring(0, num);
                value = arr[i].substr(num + 1);
                this[name] = value;
            }
        }
    }
    var Request = new getQueryUrl()
    console.log(Request.id);   
    
#### Hash参数设置

这里参数使用：

	URL+#PARAMS
	## http://localhost/event/test?#name=name&type=type
	
	location.hash = '#?name=' + PAGE_USER + '&type=shared';
	document.title ='我收到了【' + DATA_WECHAT.title + '】邀请函';
	
#### 参数处理部分

这里先判断是否获取到参数以及参数是否可用，进而判断显示的信息。

    PAGE_USER = getQuery('name');
    PAGE_TYPE = getQuery('type');

    if (PAGE_USER) {
        $('.invitation-name').html(PAGE_USER);
    } else {
        $('.invitation-name').html('Thonatos.Yang');
    }

    if (PAGE_TYPE && (PAGE_TYPE === 'me')) {
		// 显示提醒信息
    } else {
        // 显示邀请信息
    }
	