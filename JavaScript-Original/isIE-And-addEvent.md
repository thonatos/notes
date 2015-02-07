JS判断是否IE的方法

神奇的IE总是不支持一些JS的方法和事件，需要先判断其类型。

## 判断是否IE

#### 第一种

	if(!+[1,])alert("这是ie浏览器"); 
	else alert("这不是ie浏览器");
	
#### 第二种

	var navigatorName = "Microsoft Internet Explorer"; 
	var isIE = false; 
	if( navigator.appName == navigatorName ){ 
		isIE = true; 
		alert("ie") 
	}else{ 
		alert("not ie") 
	}
	
#### 第三种

	if(document.all){ 
		alert("IE6"); 
	}else{ 
		alert("not ie"); 
	}

#### 第四种

	if(window.addEventListener){ 
		alert("not ie"); 
	}else if(window.attachEvent){ 
		alert("is ie"); 
	}else{ 
		alert("这种情况发生在不支持DHTML的老版本浏览器（现在一般都支持）") 
	}

## 添加事件

#### 第一种

    function addEvent(elm, evType, fn, useCapture) {
        if (elm.addEventListener) {
            elm.addEventListener(evType, fn, useCapture);//DOM2.0
            return true;
        }
        else if (elm.attachEvent) {
            var r = elm.attachEvent(‘on‘ +evType, fn);
        )
            //IE5+
            return r;
        }
        else {
            elm['on' + evType] = fn;//DOM 0
        }
    }
    
#### 第二种

	function addEvent(element, type, handler) {
        //为每一个事件处理函数分派一个唯一的ID
        if (!handler.$$guid) handler.$$guid = addEvent.guid++;
        //为元素的事件类型创建一个哈希表
        if (!element.events) element.events = {};
        //为每一个"元素/事件"对创建一个事件处理程序的哈希表
        var handlers = element.events[type];
        if (!handlers) {
            handlers = element.events[type] = {};
            //存储存在的事件处理函数(如果有)
            if (element["on" + type]) {
                handlers[0] = element["on" + type];
            }
        }
        //将事件处理函数存入哈希表
        handlers[handler.$$guid] = handler;
        //指派一个全局的事件处理函数来做所有的工作
        element["on" + type] = handleEvent;
    }
    
    //用来创建唯一的ID的计数器
    addEvent.guid = 1;
    
    function removeEvent(element, type, handler) {
        //从哈希表中删除事件处理函数
        if (element.events && element.events[type]) {
            delete element.events[type][handler.$$guid];
        }
    }
    
    function handleEvent(event) {
        var returnValue = true;
        //抓获事件对象(IE使用全局事件对象)
        event = event || fixEvent(window.event);
        //取得事件处理函数的哈希表的引用
        var handlers = this.events[event.type];
        //执行每一个处理函数
        for (var i in handlers) {
            this.$$handleEvent = handlers[i];
            if (this.$$handleEvent(event) === false) {
                returnValue = false;
            }
        }
        return returnValue;
    }
    
    //为IE的事件对象添加一些“缺失的”函数
    function fixEvent(event) {
        //添加标准的W3C方法
        event.preventDefault = fixEvent.preventDefault;
        event.stopPropagation = fixEvent.stopPropagation;
        return event;
    }
    
    fixEvent.preventDefault = function () {
        this.returnValue = false;
    }
    
    fixEvent.stopPropagation = function () {
        this.cancelBubble = true;
    }
    
#### 第三种

	var addEvent=(function(){
        if(document.addEventListener){
            return function(el,type,fn){
                if(el.length){
                    for(var i=0;i&lt;el.length;i++){
                        addEvent(el[i],type,fn);
                    }
                }else{
                    el.addEventListener(type,fn,false);
                }
            };
        }else{
            return function(el,type,fn){
                if(el.length){
                    for(var i=0;i&lt;el.length;i++){
                        addEvent(el[i],type,fn);
                    }
                }else{
                    el.attachEvent(‘on‘+type,function(){
                        return fn.call(el,window.event);
                    });
                }
            };
        }
    })();