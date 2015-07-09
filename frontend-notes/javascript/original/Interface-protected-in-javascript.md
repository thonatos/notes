JavaScript是一个相对“较弱”的语言,所以我们通过一些手段来人为实现类似Java中的接口之类的功能。

#### modal/modal.js

	/**
 	 * Created by thonatos on 14/12/6.
 	 */

	var Modal = {
    	create: function(bundleProtected){
        	var _interface = {};
        	var _protected = {};

        	var obj = require("../components/component").create(_interface,_protected);

        	obj.init = function(){
            	_protected.init();
        	};

        	_interface.init = function(){
            	console.log("Model _interface init");
        	};

        	return obj;
    	}
	};

	exports.create = Modal.create;

#### components/component.js

	/**
 	 *	Created by thonatos on 14/12/6.
 	 */

	var Component = {
    	create : function(bundleInterface, bundleProtected){
        	var obj = {};

        	var _interface = bundleInterface || {};
        	var _protected = bundleProtected || {};

        	_protected.init = function(asyn,param){
            	console.log("Component _protected init");
            	_interface.init(param);
        	};

	        return obj;
    	}
	};

	exports.create = Component.create;
		
#### page/index.js

    var modal = require('../modal/modal').create(null);
    modal.init();
    
##### Result

	Component _protected init
	Model _interface init
    
上面的三段代码实现的功能：

* modal: _interface 提供接口给component使用。
* component: _protected 提供私有的方法给modal调用。 
* index：上述的内容最终表现为只有一个共有的init()方法可供外部使用。