提交在Github上的文档多了一个.gitignore文件，我们使用Github Api返回的结果是一个Json的数组，所以需要删除其中的元素，但是因为添加了忽略文件，并非所有的都包含这个元素，故而需要加一个判断，顺便加一个删除数组元素的方法在里面。

### JavaScript Array 

#### 参数

* 参数 size 是期望的数组元素个数。返回的数组，length 字段将被设为 size 的值。
* 参数 element ..., elementn 是参数列表。当使用这些参数来调用构造函数 Array() 时，新创建的数组的元素就会被初始化为这些值。它的 length 字段也会被设置为参数的个数。

#### 返回值

* 返回新创建并被初始化了的数组。
* 如果调用构造函数 Array() 时没有使用参数，那么返回的数组为空，length 字段为 0。
* 当调用构造函数时只传递给它一个数字参数，该构造函数将返回具有指定个数、元素为 undefined 的数组。
* 当其他参数调用 Array() 时，该构造函数将用参数指定的值初始化数组。
* 当把构造函数作为函数调用，不使用 new 运算符时，它的行为与使用 new 运算符调用它时的行为完全一样。

#### 常用方法

* slice()
* splice()
* push()
* shift()
		
### 删除元素的方法

Func：

	/* 
	*  方法:Array.remove(index) 通过遍历,重构数组 
	*  功能:删除数组元素. 
	*  参数:index删除元素的下标. 
	*/ 
	Array.prototype.remove=function(index) 
	{ 
    	if(isNaN(index)||index>this.length){return false;} 
    	for(var i=0,n=0;i<this.length;i++) 
    	{ 
        	if(this[i]!=this[index]) 
        	{ 
            	this[n++]=this[i] 
        	} 
    	} 
    	this.length-=1 
	} 
	arr = ['1','2','3','4','5']; 
	arr.remove(1); //删除index为1的元素 
	
Func：

	/* 
	*  方法:Array.remove(index) 删除元素,重构数组 
	*  功能:在原数组上修改数组. 
	*  参数:index删除元素的下标. 
	*/ 
	Array.prototype.remove=function(index) 
	{ 
    	if(isNaN(index)||index>this.length){return false;} 
    	this.splice(index,1); 
	} 
	arr = ['1','2','3','4','5']; 
	arr.remove(1); //删除index为1的元素 
	