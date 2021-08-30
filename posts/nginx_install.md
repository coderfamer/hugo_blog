---
title: centos7 安装nginx
date: 2020-12-23 16:07:32
tags: [hugo]
draft: false
---
centos7 安装nginx
====================

## 先决条件

1. root权限56
2. selinux设置

## 安装步骤
### step 1 更新存储库列表
```shell
	sudo yum -y updatee
```
### step 2 从EPEL安装外部包
&emsp; nginx不在centos标准库中，所以需要从EPEL中安装。首先安装EPEL
```shell
	sudo yum -y install epel-release
```
### step3 安装nginx
&emsp; 命令 如下
```shell
	sudo yum -y install nginx
```
### step4 启动nginx
```shell
	# 启动nginx
	systemctl start nginx
	# 查看nginx状态
	systemctl status nginx
```
### step5 设置nginx开机自启动
```shell
	systemctl enable nginx
```

### step6 配置防火墙
&nbsp; centos 7 默认开启防火墙，所以端口无法访问。可以选择关闭防火墙或者配置端口可访问
#### 方法一  关闭防火墙 
```shell
	# 关闭防火墙
	systemctl stop firewalld
	# 关闭开机自启动
	systemctl disable firewalld
```
#### 方法二  开放端口
```shell
	firewall-cmd --zone=public --permanent --add-service=http
	firewall-cmd --zone=public --permanent --add-service=https
	firewall-cmd reload
```

### step 7 检查nginx是否安装成功
&nbsp; 查看nginx所在的主机的IP地址，打开浏览器在地址栏输入  http://ip ,如果出现含有 “Welcome to CentOS”的界面，说明nginx安装成功

### nginx管理常用命令
```shell
	# 停止服务
	systemctl stop nginx
	# 重启服务
	systemctl restart nginx
	# 重新加载服务
	systemctl relaod nginx
	# 启用/禁用开机自启动
	systemctl enable/disable nginx
	# 重启nginx
	nginx -s reload
	# 停止nginx
	nginx -s stop
	# 查看配置文件路径，并检测配置文件的正确性
	nginx -t 
```





