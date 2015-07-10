Reinstall OSX - Yosemite && Make Create Install Media

##### Format Disk

- 找一个大于8G的U盘或者移动硬盘
- 创建新分区MacOS日志格式
- 记得选择GUID分区(创建分区时候在选项那边)

#### Make Create Install Media

- 下载安装镜像并放到Appliactions目录
- 执行以下命令

		sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/Install --applicationpath /Applications/Install\ OS\ X\ Yosemite.app --nointeraction
		
- Install 为磁盘名称（可以ls /Volumns/查看可用的磁盘）

#### More

- 时间问题
	
	> 格式化磁盘并重新安装的时候，可能出现“应用损坏或验证...”类的信息，记得改下时间，方法是：
	
		# 在终端中输入
		date 062202082015.30
		
		# 06 hour
		# 22 minute
		# 02 month
		# 08 day
		# 2015 year
		# 30 second