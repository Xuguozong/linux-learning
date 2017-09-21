# jvm内存分析命令
### 1. jmap及jstat安装
> Oracle JDK自带，无需安装

> OpenJDK安装（yum方式安装）:

	# 查找与openjdk对应版本的openjdk-devel开发包
	yum whatprovides '*/jmap'

	# 安装
	yum install java-1.8.0-openjdk-devel.x86_64

### 2. jmap使用
	# 查看内存使用的概要信息
	jmap pid(线程号)
	# 查看java heap信息（CMS GC的情况下可能导致进程挂起）
	jmap -heap pid
	# 查看jvm堆中对象详细占用情况
	jmap -histo pid