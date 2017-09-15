# centos7 关闭防火墙和selinux以及开端口

#### 关闭防火墙
	# 停止firewall
	systemctl stop firewalld.service

	# 禁止firewall开机启动
	systemctl disable firewalld.service

#### 设置iptables service
	yum install -y iptables-services

	# 编辑iptables文件
	vi /etc/sysconfig/iptables

	# 增加规则
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
 	
	# 重启防火墙使配置生效
	systemctl restart iptables.service

	# 设置防火墙开机启动
 	systemctl enable iptables.service

#### 关闭selinux
	# 编辑selinux文件
	vi /etc/sysconfig/selinux
	将SELINUX=enforcing改为SELINUX=disabled
	
	# 保存后退出,设置selinux状态
	setenforce 0
	
	# 获取selinux状态
	getenforce    -->Permissive状态

#### 重启系统reboot
	