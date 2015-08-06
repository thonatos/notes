
实现Div居中的方法比较多， 但浏览器兼容问题使得实现方式较为多变，这里分享一种最近使用的方法。

### HTML

	<div class="box">
		<div class="goods">Goods</div>
	</div>

### CSS(1)

	.box{
		position: relative;
		width: 200px;
		height: 100px;
		background-color: yellow;
	}
	
	.goods{
		position: absolute;
		top: 50%;
		left: 50%;
		
		-ms-transform: translate(-50%,-50%); /* IE 9 */
		-webkit-transform: translate(-50%,-50%);	/* Safari and Chrome */
		-o-transform: translate(-50%,-50%); /* Opera */
		-moz-transform: translate(-50%,-50%);	/* Firefox */
		transform: translate(-50%,-50%);
		background-color: red;
	}

### CSS(2)

	.box{
		width: 100%;
		height: 100%;
		display: table;
	}
	.goods{
		display: table-cell;
		vertical-align: middle;
		width: 300px;
		margin: 0 auto;
	}

### CSS(3)

	.box{
		width: 100%;
		height: 100%;
		position: relative;
	}
	.goods{
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		margin: 0 auto;
	}

