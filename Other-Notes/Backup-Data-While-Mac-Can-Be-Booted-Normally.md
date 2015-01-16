Mac无法启动情况下的Single-User-Mode数据备份，记录备用。

- 开机启动
	
		command+s
	
- 加载根分区

		/sbin/mount -uw /
		# 根分区挂载
		
		/sbin/fsck -fy 
		# 磁盘检查

- 列出磁盘信息

		ls /dev/disk*
		# 一般自己的移动硬盘为disk1s1
		
- 挂载移动硬盘

		mkdir /Volumns/mobile_driver
		/sbin/mount_msdos /dev/disk1s1 /Volumns/mobile_driver
		
- 备份个人数据

		cd /Volumns/mobile_driver
		mkdir backup_mac_data
		
		cp -R /your_dir_or_file ./
		
尝试过其他方式，这种方法是最为简单简洁的，但是在某些情况下，这样的方式不生效，那就使用可移动的光盘进行命令行操作，方法其实是一样的，挂载移动硬盘，复制数据。