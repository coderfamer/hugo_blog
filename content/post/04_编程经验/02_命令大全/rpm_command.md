---
title: "rpm 常用命令"
date: 2022-12-12T11:11:30+08:00
categories:
  - 命令大全
tags:
  - rpm 
  - linux
draft: false
---

``` 
rpm -ivh demo.rpm   	# 安装
rpm -Uvh demo.rpm       # 升级软件包
rpm -qa | grep "name" 	# 查找某个包
rpm -e demo.rpm     	# 卸载
rpm -qi  demo.rpm   	# 查看包信息
rpm -ql  demo.rpm       # 查看包安装路径
```



