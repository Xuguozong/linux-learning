# tomcat开机自启动脚本和监控脚本编写

### 1.tomcat启动脚本(针对压缩包方式安装的tomcat)
	#!/bin.bash
	
	# chkconfig: 2345 96 14 
	# Default-Start: 2 3 4 5 
	# Default-Stop: 0 1 6 
	# description: 

	date=`date '+%Y-%m-%d'`
	# tomcat路径
	CATALINA_HOME=/home/ec2-user/tomcat7		
	# 自定义变量
	NAME=tomcat7
	
	export CATALINA_HOME

	# 判断脚本命令参数
	case "$1" in
	start)
	  echo -n "[${date}] ${NAME} start: "
	  ${CATALINA_HOME}/bin/catalina.sh start
	  echo "[${date}] ${NAME} start finished: "
	  ;;
	stop)
	  echo -n "[${date}] ${NAME} stop: "
  	  ${CATALINA_HOME}/bin/catalina.sh stop
	  echo "[${date}] ${NAME} stop finished； "
	  ;;
	reload|restart)
	  $0 stop
	  echo "[${date}] ${NAME} stop: "
	  sleep 2
	  $0 start
	  echo "[${date}] ${NAME} stop: "
	  ;;
	log)
	  tail -f ${CATALINA_HOME}/logs/catalina.out
	  ;;
	*)
	  echo "Usage: [start|stop|reload|restart|log]"
	  exit 1
	esac
	exit 0

> 将以上脚本复制到/etc/init.d目录下并添加执行权限

	sudo chmod +x /etc/init.d/tomcat
	# 添加到开机启动项
	sudo chkconfig --add tomcat
	# 或者
	sudo chkconfig tomcat on
	# 查看是否添加到启动列表
	sudo chkconfig --list tomcat

---

### 2.tomcat服务监控脚本

	#!/bin/bash

	# tomcat路径
	CATALINA_HOME=/opt/tomcat7

	# 获取tomcat进程号
	tomcat_pid=$(ps -ef | grep tomcat7 | grep -w 'tomcat7' | grep -v 'grep' | awk '{print $2}')	

	# tomcat启动
	tomcat_start=${CATALINA_HOME}/bin/catalina.sh start

	# tomcat停止
	tomcat_stop=${CATALINA_HOME}/bin/catalina.sh stop

	# 定义要监控的tomcat服务地址
	url=127.0.0.1:8080
	
	# 日志输出
	log_path=/var/logs/tomcat-monitor.log
		
	# 监控函数
	monitor(){
	  echo "[INFO]开始监控tomcat7...[ `date '+%F %H:%M:%S` ]"
	  http_code=$(curl -I -m 10 -o /dev/null -s -w %{http_code}"\n" ${url})

	  if [ ! -n ${http_code} -eq 200];
	  then
		echo "[INFO]${url}返回码：${http_code},正常运行..."
		echo
	  elif [ ! -n ${tomcat_pid} ];
	  then
		echo "[WARN]tomcat停止运行,tomcat7进程为空,页面返回码为:${http_code},3秒后开始重新启动...
		${tomcat_start}
		echo
	  else
	 	echo "[ERROR]tomcat7停止运行,tomcat7进程不为空,3秒后重新启动..."
		kill -9 ${tomcat_pid}
		${tomcat_start}
		echo
	  fi
	}
	
	# 写入日志文件
	monitor >> ${log_path}

---

### 3.定时任务脚本
	HOME=/opt/tomcat
	*/5 * * * * ./monitor.sh

> 通过crontab加入到定时任务

	# 命令格式
	crontab [-u user] file
	crontab [-u user] [ -e | -l | -r ]
	# 命令参数
	-u user:指定某个用户
	file:cron表达式文件
	-e:手动编辑cron表达式
	-l:显示某个用户的crontab文件内容
	-r:删除用户的/var/spool/cron目录中的crontab文件
	-i:在删除用户的crontab文件时给确认提示