# Linux 命令积累

### top 命令

	top --->查看资源使用状况
	top -p pid --->查看特定进程的资源使用状况
	top shift + > --->调整，按其它选项排序

---

### 指定分隔符(:)分列显示

	| column -t -s:

---

### 回到上个目录

	cd -

---

### 对某项命令默认yes

	yes | yum update

---

### rpm 命令

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

---

### curl 命令
	# 下载到本地并重命名
	curl -o newname.file http://......

	# 默认名
	curl -O url1 -O url2

	# 重定向
	curl -L url

	# 断点续传
	curl -C url
	
	# 限速
	curl --limit-rate 1000B url

	# 下载指定日期内修改过的文件
	curl -z 21-Dec-11 url

	# 授权
	curl -u username url
	
	# 设置代理
	curl -x ip:port url

	# POST 请求
	curl -u username --data "para1=value1&para2=value2" url
	curl --data-urlencode url(自动转义特殊字符)
	curl --data @filename url(上传文件)
	curl --form "fileupload=@filename" url(上传文件)

	# 删除
	curl -X DELETE url

---

### du 命令
	
	# 查看文件或文件夹大小
	du -hs -m /opt
	
---
### iptables开放端口
	iptables -I INPUT -p tcp --dport [端口号] -j ACCEPT

---
### yum
	# 查看已安装的软件
	yum list installed
	# 查看统一软件包的多个版本
	yum list [包名] --showduplicates | sort -r
---
### split 分割大文件
	# 分割成100M的小文件
	split -b 100M catalina.out
---
### ssh服务器间免密登录
    	# 生成秘钥
	ssh-keygen -t rsa 
	# 传输公钥 
	scp ./id_rsa.pub user@ip:/home/user/.ssh/authorized_keys  确保authorized_keys的权限是664
---
### 查看进程内有多少线程
	ps -o nlwp [pid]
	nlwp - number of light-weight process
---
### 查看进程内的线程详情
	top -H -p [pid]
---
### 查看linux系统的tcp连接情况
	# overiew
	netstat -an | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
	# 每隔1s监控TIME_WAIT的tcp数量
	watch -n 1 "netstat -nt | grep TIME_WAIT | wc -l"
---
### 查找文件
	# find  根据文件属性进行查找，如文件名、文件大小、所有者、所属组、是否为空、访问时间、修改时间等
	# 根据文件名  .标识当前路径
	# find [path] -name [file name]
