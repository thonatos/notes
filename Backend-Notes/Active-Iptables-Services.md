> * title: CentOS 7 开启 iptables禁止外部ip访问内部端口
> * date: 2015-06-19 16:31:05

我们使用nginx作为前端代理处理网络访问请求，为了安全性考虑，要尽量访问外部ip访问一些内部端口例如mongodb的数据库27017，那么做法如下：

	yum install iptables-services
	

然后修改配置文件：

	/etc/sysconfig/iptables

内如如下：

	# sample configuration for iptables service
	# you can edit this manually or use system-config-firewall
	# please do not ask us to add additional ports/services to this 	default configuration
	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [0:0]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p icmp -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 22022 -j ACCEPT
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 22021 -j ACCEPT
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT
	-A INPUT -j REJECT --reject-with icmp-host-prohibited
	-A FORWARD -j REJECT --reject-with icmp-host-prohibited
	COMMIT
	
	
最后重新启动iptables服务器即可：

	systemctl restart iptables.service
