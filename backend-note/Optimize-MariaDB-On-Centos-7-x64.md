> * title: CentOS 7.0 x64下MariaDB异常结束的解决方法
> * date: 2014-08-20 02:36:10

团队使用的是DigitalOcean的Vps，并且使用了LEMP作为运行环境，

团队主站是使用WordPress作为主程序来做支持，不过近期经常出现“建立数据库连接时出错”这个错误提示，

大概推测应该是数据库出问题了，所以我们现在先登录服务器查看一下错误代码。

#### ssh连接过去先连接过去，然后输入：

    sudo systemctl status mariadb.service
    
    ## 这时候可以看到一些错误提示:
    140819 14:06:14 mysqld_safe Logging to '/var/log/mariadb/mariadb.log'.
    140819 14:06:14 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql

    ## 这里可以看到，log存储在/var/log/mariadb/mariadb.log里面，接着我们继续去查看一下问题：
    sudo nano /var/log/mariadb/mariadb.log
    
    ## 然后可以看到里面比较重要的问题是出现在了下面这句话里面：
    140819 14:06:14 InnoDB: Fatal error: cannot allocate memory for the buffer pool
    
    ## 这里问题就比较明确了，内存不足，故而无法分配资源。

#### 可行的解决方法大概有这么三种

* 增加物理内存，使用的是DG的服务器，所以就直接升级配置即可。
* 创建Swap分区，这种方式应该算是比较好的解决方法，使用的命令如下：

```
	## 使用Root权限，直接sudo -i吧
    dd if=/dev/zero of=/swap.dat bs=1024 count=524288
    ## 524288=512*1024也就是说分配了512M的交换分区
    mkswap /swap.dat
    swapon /swap.dat
    ## 然后查看下效果
    free -m
    ## 会出现下面的内容，也就是说已经成功了
                 total       used       free     shared    buffers     cached
    Mem:           490        453         37          1          1         22
    -/+ buffers/cache:        430         60
    Swap:          511         48        463
    ## 这样还不行，我们需要继续让系统自动挂载swap分区，编辑/etc/fstab并添加一行：
    /swap.dat      swap    swap      0       0 
    ## 现在基本已经解决问题了，直接启动mariadb应该是没有问题了：
    sudo systemctl start mariadb.service
```

* 修改Mysql内存池大小，编辑/etc/my.cnf并修改如下内容：

```
	[mysqld]
	innodb_buffer_pool_size=64M
```

最后打开网站吧，应该可以看到内容了。
这里建议重启下服务器，当然，这不是必需的，也可以稍后重启。