> * title: D2G中国电信中文显示
> * date: 2011-07-21 19:45:10

今天又兄弟问道如何能够使我们的机器显示中国电信，这里我做下说明。  
主要修改的文件就是framework-res.apk

#### 准备工作：

一，安装java环境，这里我就不多说了，去百度一下。

二，下载工具包，android-apktool

	http://code.google.com/p/android-apktool/

下载apktool-1.0.0.tar.bz2 和apktool-install-windows-2.1_r01-1.zip

解压apktool.jar 到 C:/Windows文件夹下

解压apktool-install-windows.zip到任意文件夹

开始-运行-输入CMD回车，用cd命令转到刚刚解压apktool-install-windows所在的文件夹，输入

	apktool

出现一些命令说明即成功安装。

#### 修改过程：

一，首先提取你的framework-res.apk（记得备份一下，电脑上用到的有两处）

二，在解压后的apktool目录下（cmd下cd命令切换....）

	apktool d framework-res.apk framework    #反编译 framework-res.apk 到文件夹framework
	
三，切换到framework目录下找到eri.xml文件，然后修改其中的China Telecom（全部替换好了，在刚开始的地方，总共三处）

四，重建文件。

	apktool b framework framework.apk 编译 文件夹framework 到文件framework.apk
	
并将其中的eri.xml提取出来，备用。

五，现在打开你的备份文件，（之前让备份了的那个），用winrar打卡，然后找到eri.xml，将刚才从重新编译的framework.apk中提取的那个eri.xml直接拖进去。选择为存储哦。

六，现在就是很简单也很容易忘记的地方了，不知多少人变砖就是因为这个。。。。。。

将framework.apk放进内存卡根目录并更名为framework-res.apk

先复制到system目录并修改权限，为：

	xx-

	x--

	x--

然后选择移动到framework目录覆盖已有文件。（记得先在root文件浏览器上面选择为读写模式）

好了，重启吧，现在电信卡应该可以显示“中国电信”字样了。