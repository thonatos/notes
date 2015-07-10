
MT.NoteAnd.Documents && MT.NOTES

一个在试验前后端分离方案时候顺便搭建起来的类博客站点。

#### About

我们曾经就前后端分离做过很多的讨论，停留在讨论阶段的讨论始终没有实际意义，讨论的最终目的是用于实际使用，故而将讨论变为实际可行的应用才是我们要达到的效果。

作为一个使用WP长达六年的屌丝前端，对WP有欣赏也有不满，安装便捷，插件丰富，文档齐全让使用变得简单方便；但与此同时，也变得更加“臃肿”，我始终相信博客的本质在于内容，功能最多只是锦上添花，内容才是核心。

所以最终还是在试验的时候放弃了WP，用Github提供的API搭建了这个类博客站，并替换了原先的个人站。

#### Why

* 为什么不用数据库？

	为什么我这里不用数据来存储文章内容的原因在之前的一篇文章中已经说明过 —— 这个小程序的核心是试验前	后端分离方案；所以程序本身不包含数据和信息的存储与读写处理。
	
* 为什么用NodeJS？

	个人倾向算是其中一个原因，过去对后端的依赖过大，往往新加C（Controller）或者V（View）都需要后端帮忙，自主性太差，频繁的找后端测试什么的实在是很蛋疼，所以，干脆把那部分权利剥夺了吧，我们自己来写，让后端专心的去弄他得数据好了~
	
* 为什么用中间件？

	曾经试验过SPA解决方案，但是SPA在SEO和加载的速度上，存在较大的问题，另外从后端接受数据的时间受网络影响比较大，尤其是涉及数据运算的时候，等待时间过长，在等待中页面需要显示loading或者空白，给人的感觉是程序bug，体验上得优化虽然能解决问题，但始终不是最佳方案。
	
* 有什么优点？
	* 自主性（我们有足够的权限做自己想做的事情）
	* 独立性（再也不用一起部署测试了！后端去部署后端，前端去部署前端，just do what we want ! ）
	* 其他..（前后端分离的优缺点还是自己去找吧）
	
* 有什么问题？
	* 试验项目（这是一个实验项目，知乎大神说没有需要还是不要去尝试~）
	* 未知问题（听说未来会遇到一些列的瓶颈什么的，但我认为有问题不要紧，去解决就好~）

	
#### How

* 文档托管

	Documents/Posts存放在Github，格式为Markdown或者其他（html也可以）
	
* 运行环境

	程序依赖于NodeJS，基于ExpressJS搭建，本质上就是一个常规的基于NodeJS的WebProject，区别在于未使用数据库进行数据的存储与读取，将Model的职责分配给后端，在这个项目中，是将该职责转交到Github Api，如果是在公司项目中，这里会交给后端的API进行处理。
	
#### Changelog

- [https://github.com/MT-Libraries/MT-Notes](https://github.com/MT-Libraries/MT-Notes)
- [https://coding.net/u/thonatos/p/MT-Notes/git](https://coding.net/u/thonatos/p/MT-Notes/git)

#### Thanks

* SeaJS     模块化开发
* GulpJS    流式构建
* Github    文档API服务
* MongoDB   数据库
* AngularJS MVVM前端框架
* ExpressJS 网站核心程序
* PassportJS 网站认证模块

感谢用到类库作者的无私奉献以及Github。

#### License

The MIT License (MIT)

Copyright (c) 2014 [@Thonatos.Yang](http://github.com/thonatos)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
