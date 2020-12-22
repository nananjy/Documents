# iptables 防火墙

## Unit iptables.service could not be found.
- 问题描述
> 查看防火墙状态
<br/>[root@localhost opt]# `service iptables status`
<br/>Redirecting to /bin/systemctl status iptables.service
<br/>Unit iptables.service could not be found.
- 解决方案
> 在CentOS 7或RHEL 7或Fedora中防火墙由firewalld来管理，当然你可以还原传统的管理方式，或则使用新的命令进行管理。
<br/>本文中将<b>还原传统的管理</b>方式。
1. cd /etc/sysconfig
2. ls -l
<br/>没有查看到iptables文件，但存在ip6tables-config和iptables-config，本文中的linux为RHEL7.4，默认没有iptables文件
3. 安装iptables-services
<br/>`yum install iptables-services`
4. 启动iptables
<br/>`systemctl enable iptables` 设置链接
<br/>`systemctl start iptables`
5. 查看、打开、关闭防火墙
> system iptables status
<br/> system iptables start
<br/> system iptables stop
