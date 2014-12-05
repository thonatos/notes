> * title: 编辑并重建odex文件
> * date: 2011-09-03 21:20:27


从现在开始不再发布ROM，只发布一些常用的教程，这是系列教程之一。
修改系统文件，未Deodex的系统文件不易修改，修改了，也有很多人不知道如何重建。

#### 预准备

* 电脑必须安装了Java环境（不会配置的在我博客搜索）
* 首先复制手机的framework文件夹内的全部内容到电脑
* 并在在该文件夹中放入编译的工具，Basksmali和Smali、最好再加入一个cmd.exe
* 然后还需要提取你需要修改的APK和ODEX文件到该目录
* 所需的软件我会在日志的末尾处放出来，大家也可以自行搜索

【合并APK和ODEX】

1.通过odex生成class文件

	java -jar baksmali.jar -x ex.odex

2.通过class生成classes.dex 文件。
	
	java -Xmx512M -jar smali.jar out -o classes.dex

3.将classes.dex放到apk文件

因为apk是zip的mime编码类型，直接将dex拖入到apk改名为zip的压缩包中即可

4.现在最好将该apk重新签名一下，这样可以方便一些

【反编译】

1.反编译APK文件

	apktool d ex.apk ex
	
2.然后在ex目录里面修改你打算修改的内容

3.重新打包APK

	apktool b ex ex-new.apk
	
【生成ODEX】

1.将tool目录下的dexopt-wrapper复制到手机systembin目录下，并修改好权限

	xxx
	x-x
	x-x

2.将重建的ex-new.apk复制内存根目录
3.开启手机调试模式，并修改连接模式“仅充电”
4.在tool目录下的cmd.exe中输入并等待出现$

	adb shell
	
5.输入su命令，记得要授权，成功后出现#（已经是#的就不用管了）
6.输入

	cd /sdcard/
	
7.输入
	
	dexopt-wrapper ex-new.apk ex-new.odex
	
8.现在如果看到SUCCESS字样应该就是成功了，对比与原来的ODEX文件差异，相近则成功

注意：

1.现在在内存卡根目录更名ex-new.odex为ex.odex并放回原来的位置就可以，权限必须与原来相同

2.重建的apk不要放进原来提取的位置，你应该提取相应的.xml文件替换到最初提取并备份的apk里面.

【相关下载及补充说明】

该部分说明到此为止，下一部分随后更新。
好了，现在修改的方法就是这样了，相关软件的下载，请自行搜索。
