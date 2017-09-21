# CentOS 7升级内核

### 1.设置yum源
	RHEL-7，SL-7，CentOS-7 源： rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
	RHEL-6，SL-6，CentOS-6 源： rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
	RHEL-5，SL-5，CentOS-5 源： rpm -Uvh http://www.elrepo.org/elrepo-release-5-5.el5.elrepo.noarch.rpm

### 2.安装fastmirror
	yum install yum-plugin-fastmirror

### 3.安装kernel
	yum --enablerepo=elrepo-kernel install kernel-ml

	# 将kernel-ml设为第一启动后重启
	grub2-set-default 0

### 4.开启BBR TCP
	echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
	echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

	# 查看BBR TCP是否开启
	sysctl net.ipv4.tcp_available_congestion_control
	sysctl net.ipv4.tcp_congestion_control

	# 查看tcp_bbr模块是否加载
	lsmod | grep tcp_bbr