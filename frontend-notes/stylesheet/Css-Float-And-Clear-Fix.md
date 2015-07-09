Css定位中除了比较常见的有绝对、相对定位，同时比较常见的还有float属性。

float的使用，通常需要注意的就是清除浮动。

#### 默认布局

HTML页面的标准文档流(默认布局)是：从上到下，从左到右，遇块(块级元素)换行。

#### 浮动布局

给元素的float属性赋值后，就是脱离文档流，进行左右浮动，紧贴着父元素(默认为body文本区域)的左右边框。

而此浮动元素在文档流空出的位置，由后续的(非浮动)元素填充上去：

* 块级元素: 直接填充上去，若跟浮动元素的范围发生重叠，浮动元素覆盖块级元素。
* 内联元素: 有空隙就插入。

> float：prop

	① left ：元素向左浮动。
	② right ：元素向右浮动。
	③ none ：默认值。
	④ inherit ：从父元素继承float属性。
	
	
当日常使用中我们通常会选择给元素添加float，然后一疏忽就会发现父元素的高度居然是0？
这个是因为使用float需要清除浮动，才能让其后的元素继续遵循文档流的默认布局。

举例来说：

	<div class="box">
		<div class="block-a">BlockA</div>
		<div class="block-b">BlockB</div>
		<div class="clear-fix">
	</div>
	
	.box{}
	.block-a,
	.block-b{
		float:left;
	}
	.clear-fix{
		clear:both;
	}
	
上面的例子中，BlockA与BlockB是紧挨着的向左浮动的，并且.box的高度是正常且不为0的，但是你可以尝试下去掉.clear-fix的clear之后，样式不变，但是，.box的高度变为0了，在后续中如果需要获取.box的高度，显然就不行了，所以，在使用float属性之后，务必记得要清除浮动。

所以在更常见的使用中会选择这样使用：

	<div class="box">
		<div class="block-a">BlockA</div>
		<div class="block-b">BlockB</div>
	</div>
	
	.box{}
	.box:before{
		content:' ';
		display:table;
	}
	.box:after{
		content:' ';
		display:table;
		clear:both;
	}
	
	.block-a,
	.block-b{
		float:left;
	}


#### 参考文档

* http://www.w3school.com.cn/css/css_positioning_floating.asp