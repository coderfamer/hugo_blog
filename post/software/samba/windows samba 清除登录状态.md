---
title: windows 清除 samba 登录状态
date: 2022-03-14 15:07:32
categories: ['']
tags: ['', '', '']
draft: false

---

# windows 清除 samba 登录状态

## 简介

windows 登录 samba 共享目录之后，会保存当前用户的缓存，每次访问之后就无需再次输入账户密码，想要使用其他账户就需要把当前账户状态清理掉

## 操作

### 方法一

重启电脑，如果保存用户名密码重启电脑不会清理账户状态

### 方法二

命令行清理

打开 cmd  命令行窗口

查看所有连接

```shell
net use 
```

清除某个连接

```powershell
net use <remote-name> /del 
```

清除所有连接

```powershell
net use * /del
```



