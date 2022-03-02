---
title: centos 7 配置 samba client
date: 2021-08-26 16:07:32
categories: [software, samba]
tags: [samba]
draft: false

---
# Samba - 如何配置 samba 客户端
---
SMB（Server Message Block）又称 CIFS(Common Internet File System),一种应用层网络传输协议。
本文介绍 centos 7 如何访问 smb 服务器的共享目录


## 安装 samba 软件
---
```shell
yum -y install samba samba-client cifs-utils
```
注意: 推荐安装 samba 软件

创建挂载目录
```shell
mkdir -p /mnt/projects
```
确认是否能够连接到服务器，以及可以访问的共享目录
```shell
# sambalicent -L //samba-server  -U samba-username
smbclient -L //samba-server -U samba
Enter SAMBA\samba_user1's password:
Domain=[LOCALHOST] OS=[Windows 6.1] Server=[Samba 4.6.2]

	Sharename       Type      Comment
	---------       ----      -------
    homes           Disk      Home Directories
	print$          Disk      Printer Drivers
	shared          Disk      Shared Directories
    projects        Disk      Shared Directories
	IPC$            IPC       IPC Service (Samba server samba-storage)
	samba           Disk      Home Directories
Domain=[LOCALHOST] OS=[Windows 6.1] Server=[Samba 4.6.2]

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
    SAMBA                LOCALHOST
```
手动挂载测试
```shell
mount -t cifs -o user=samba-username,password=pw123  //samba-server/projects /mnt/projects
```
挂载成功之后，使用 **df -h** 命令可查看目录映射
解挂命令
```shell
unmount /mnt/projects
```
添加开机自动挂载
在 **/etc/fstab** 文件里面添加类似命令
```shell
//samba-server/projects /mnt/projects cifs username=samba,password=pw123,soft,rw 0 0
```
测试
```shell
mount -a
```

## 参考文档
[Samba – How to set up a Samba client on CentOS/RHEL 7](https://codingbee.net/rhcsa/samba-how-to-set-up-a-samba-client-on-centos-rhel-7)
