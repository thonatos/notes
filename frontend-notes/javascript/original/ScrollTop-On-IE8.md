ScrollTop On IE8

#### Question

为了给用户更好的体验，我们用scrollListener监听页面滚动，并判断当前页面的位置，并给对应的menu设置active状态，于此同时，当单击menu时，我们需要将页面滚动到对应的位置，这里，在IE8下面的测试，发现了一个问题。

    function initNavJump() {
    
        var offset = 64;
    
        $('.top-nav-ul li a').click(function () {
            var $nav = $(this);
    
    
            var href = $nav.attr('href');
            var $target = $(href);
            var scrollTop = $target.position().top - offset;
    
            if (href.indexOf('http') == -1) {
    
                $('body').stop().animate({
                    scrollTop: scrollTop
                });
            }
            
            return false;
        });
    }
    
这样在FF/CHROME（以及高版本的IE）下是正常的，但在IE8下面是不同的，方法是：

- 设置方法：

		# FF、IE8        
		document.documentElement.scrollTop = 100;
	
		# chrome
		document.body.scrollTop = 100;
		
- 取值方法：

		scrollTop = document.documentElement.scrollTop + document.body.scrollTop;
		
#### Solution

- 最简单的解决方法

		$('body,html').stop().animate({
			scrollTop: scrollTop
		});
- 相对这个，还可以判断浏览器版本什么的，喜欢的话自己去玩吧~