> * title: CentOS 7.0 x64更改Hostname
> * date: 2014-09-09 03:31:05

团队更换域名，需要对Vps中的hostname进行更改，看了网上的很多说法，都是说修改两处内容：

    /etc/sysconfig/network
    /etc/hosts

其实CentOS 7.0 中对于hostname的更改已经更换了命令，

故而以前的这种方式仅仅支持老的版本如CentOS 6，在7中已经更换为另一种方式：

    hostnamectl set-hostname name

输入上述命令即可更改hostname，重启后在terminal输入：

    hostname

就可以看到已经更改成功了。