之前是私有项目，为了速度，代码放在coding上面，现在决定开源，所以要放到github上面了。

#### 换仓

一种不错的方法：

	git branch -r 
	# 如果结果显示origin/master，那么就可以直接进入下一步了。
	
	git remote set-url origin remote_git_address
	# 这里的地址换成你的repo的地址
		
	git push origin master
	
还有一种方法：

	git commit -m "Change repo." 
	# 先把所有为保存的修改打包为一个commit
	
	git remote remove origin 
	# 删掉原来git源
	
	git remote add origin [YOUR NEW .GIT URL] 
	# 将新源地址写入本地版本库配置文件
	
	git push -u origin master 
	# 提交所有代码

#### 优点

- 第一种方法，的好处就是所有的历史记录都会保留下来

- 第二种方法，注释说的很明白了，不过我没测试，代码丢了不关我事情啊~