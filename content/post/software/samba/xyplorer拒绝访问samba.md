---
title: xyplorer 访问 samba 时候拒绝访问
date: 2021-05-06 20:07:32
categories: [software, samba]
tags: [samba, xyplorer]
draft: false

---
# xyplorer 访问 samba 时候拒绝访问
---

## 问题
Ubuntu 配置好 samba 之后，自带文件管理器可以访问 samba 共享目录，但是用 xyplorer 访问时候， xyplorer 提示拒绝访问
## 原因
本地文件管理器在访问 samba 共享目录时候会提示输入账户名密码，而 xyplorer 不会与自带文件管理器同步密码，所以导致拒绝访问
## 解决办法
只需要在 xyplorer 中做个网络映射即可
### 操作步骤
Tools -> 特殊工具 -> 映射网络驱动器
任选一个盘符之后，输入 samba 创建的账户名即可

