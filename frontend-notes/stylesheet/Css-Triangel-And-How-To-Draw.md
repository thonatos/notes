Css的一些使用技巧可以帮助我们快速的创建一个兼容各个浏览器的三角形。

#### 特殊符号

最简单的用法莫过于使用特殊符号，然后设置颜色了。

	<span class="triangle">▲</span>
	
	.triangle {
		color: #BADA55;
		text-shadow: 0 0 20px black;
	}
	
#### 使用图片

这个就不说了，都懂。

#### Css旋转

利用transform属性对正方形进行旋转，也可以画一个三角形出来，但是兼容性难做保证，尤其在某些“神器”下面，那表现就是...

	<div class="triangle-with-shadow"></div>
	
    .triangle-with-shadow {
         width: 100px;
         height: 100px;
         position: relative;
         overflow: hidden;
         box-shadow: 0 16px 10px -17px rgba(0,0,0,0.5);
    }
    .triangle-with-shadow:after {
         content: "";
         position: absolute;
         width: 50px;
         height: 50px;
         background: #999;
         transform: rotate(45deg); /* Prefixes... */
         top: 75px;
         left: 25px;
         box-shadow: -1px -1px 10px -2px rgba(0,0,0,0.5);
    }  
    
#### Css Border

css的border大法可以说是兼容性最好，同时也是最方便的实现方式，当然了，要做实现一些特殊角度的三角形，还是需要稍微计算一下的，学渣不讨论计算问题，自行研究吧~

	.triangel{
		width:0;
		height:0;
		border-left:10px solid #ffff00;
		border-right:10px solid #ff00ff;
		border-top:10px solid #00ff00;
		border-bottom:10px solid #0000ff;
	}
	
这里可以看出来是一个正方形通过对角线相连分成了4个三角形。
然后通过调整对边的长度可以改变它的形状，我建议哦，要是想实现特定角度的三角形，还是画个正方形什么的，自己去算算吧，勾三股四什么的就好好用用吧...

**消除锯齿**

	-webkit-font-smoothing: antialiased;
	
#### Example

更多更神奇的形状可以参考这里：

* http://css-tricks.com/examples/ShapesOfCSS/