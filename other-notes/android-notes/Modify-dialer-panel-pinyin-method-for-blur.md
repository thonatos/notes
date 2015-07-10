> * title: Blur拨号面板拼音检索修改方法
> * date: 2011-10-10 13:00:48

#### 预准备

* 电脑必须安装了Java环境（不会配置的在我博客搜索）
* 首先复制手机的framework文件夹内的全部内容到电脑
* 并在在该文件夹中放入编译的工具，Basksmali和Smali、最好再加入一个cmd.exe
* 然后还需要提取你需要修改的APK和ODEX文件到该目录
* 所需的软件我会在日志的末尾处放出来，大家也可以自行搜索

#### 修改文件说明

主要修改文件：BlurDialer.apk BlurDialer.odex

主要修改位置：SmartDialerAdapter$4.smali

#### 关键代码

源代码：

	const-string v5, "( REPLACE(UPPER(display_name), ' ','') GLOB ? OR UPPER(display_name) GLOB ? OR REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(UPPER(data1), '-',''),'.',''),' ',''),'(',''),'/',''),')','') GLOB ?)"
	
修改后代码：

	const-string v5, "( REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(UPPER(sort_key_alt),SUBSTR(display_name,1,1),''),SUBSTR(display_name,2,1),''),SUBSTR(display_name,3,1),''),SUBSTR(display_name,4,1),''),' ','') GLOB ? OR UPPER(display_name) GLOB ? OR REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(REPLACE(UPPER(data1), '-',''),'.',''),' ',''),'(',''),'/',''),')','') GLOB ?)"
	
#### 反编译

1.反编译APK文件

	apktool d ex.apk ex
	
2.然后在ex目录里面修改你打算修改的内容

3.重新打包APK

	apktool b ex ex-new.apk
	
##### 生成ODEX

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

#### 相关下载及补充说明

该部分说明到此为止，下一部分随后更新。
好了，现在修改的方法就是这样了，相关软件的下载，请自行搜索。