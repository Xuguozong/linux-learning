# Linux 命令积累

> top 命令

	top --->查看资源使用状况
	top -p pid --->查看特定进程的资源使用状况
	top shift + > --->调整，按其它选项排序

---

> 指定分隔符(:)分列显示

	| column -t -s:

---

> 回到上个目录

	cd -

---

> 对某项命令默认yes

	yes | yum update

---

> rpm 命令

	# 安装一个软件包
	rpm -ivh

	# 升级软件包
	rpm -Uvh

	# 卸载软件包
	rpm -e --nodeps(强制卸载)

	# 查询是否被安装
	rpm -q 软件名

	# 查询软件信息
	rpm -qi 软件名
 	
	# 列出软件包有哪些文件
	rpm -ql 软件名

	# 列出文件属于哪个软件
	rpm -qf 文件名
	
	# 列出所有安装的软件
 	rpm -qa
