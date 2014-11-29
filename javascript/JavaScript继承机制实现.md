
任何的类都可以作为基类，选定基类后，就可以创建他的子类了，当基类不能直接使用且通过基类给子类提供通用的函数的方式则可以认为是抽象类，js的继承的实现方法不止一种，常用的是对象冒充，call()，和apply()三种方式。

##### 对象冒充

	function ClassBase(param){
	    this.param = param;
	    this.log = function () {
	        console.log(param);
	    }
	}
	function ClassChild(param,param1){
	    this.newMethod = ClassBase;
	    this.newMethod(param);
	    delete  this.newMethod;
	
	    this.param1 = param1;
	    this.log1 = function(){
	        console.log(param1);
	    }
	}
	var objA = new ClassBase("p");
	var objB = new ClassChild("p1", "p2");
	objA.log(); //p
	objB.log(); //p1
	objB.log1();//p2
	
#### Call()

	function ClassBase(param){
    	this.param = param;
    	this.log = function () {
        	console.log(param);
    	}
	}
	function ClassChild(param,param1){
	   //this.newMethod = ClassBase;
	   //this.newMethod(param);
	   //delete  this.newMethod;
	   ClassBase.call(this,param);
	   this.param1 = param1;
	   this.log1 = function(){
	       console.log(param1);
	   }
	}
	var objA = new ClassBase("p");
	var objB = new ClassChild("p1", "p2");
	objA.log(); //p
	objB.log(); //p1
	objB.log1();//p2
	
#### Apply()

	function ClassBase(param){
	    this.param = param;
	    this.log = function () {
	        console.log(param);
	    }
	}
	function ClassChild(param,param1){
	    //this.newMethod = ClassBase;
	    //this.newMethod(param);
	    //delete  this.newMethod;
	    ClassBase.apply(this,arguments);
	    this.param1 = param1;
	    this.log1 = function(){
	        console.log(param1);
	    }
	}
	var objA = new ClassBase("p");
	var objB = new ClassChild("p1", "p2");
	objA.log(); //p
	objB.log(); //p1
	objB.log1();//p2