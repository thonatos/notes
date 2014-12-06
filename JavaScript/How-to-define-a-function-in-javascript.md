> * title: JavaScript 函数声明与函数赋值
> * date: 2014-09-16 17:56:09

Js声明式函数与赋值式函数在日常使用中是比较多的，这里是摘抄了StackOverflow上得解释。

#### 函数赋值

	var func = function(){
		// do something
	}

#### 函数声明

	function func(){
		// do sothing
	}
	
#### 差异

除了语义上的区别，还在于JavaScript解释器对它解析过程的一个特性——hoist（提前？）

> Function declarations and function variables are always moved (‘hoisted’) to the top of their JavaScript scope by the JavaScript interpreter.

换句话说函数声明被解析之后是这样的：

	var func;
	// do something
	
	function func(){
		// do something
	}
	
就是说在解析过程中，variable instantiation（变量实例化）时候，（这在执行代码run time之前进行），var都会被提前，
所有var定义的变量都被初始化为undefined，然后再进行赋值，而function declaration定义的函数将被正常初始化，那么对应的，函数赋值的方式，func只能对后面的代码可见，而函数声明的方式，则是对于所在的scope可见。

#### 相同

尽管在变量实例化的时候顺序与可见性不同，但是在run time时候，都是普通的函数引用，所以：

* 最终在代码运行阶段，都变成global object的一个non-deleteable属性
* 都可以被重新指向。比如：functionOne = functionTwo; functionTwo = null;

#### 原文

[how-to-define-a-function-in-javascript.md](https://github.com/simongong/js-stackoverflow-highest-votes/blob/master/questions1-10/how-to-define-a-function-in-javascript.md)