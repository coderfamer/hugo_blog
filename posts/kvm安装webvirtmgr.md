---
title: kvm 安装 webvirtmgr
date: 2020-04-13 23:07:32
draft:  false

---

# kvm 安装 webvirtmgr
---

## 前言
---
  熬了十几个日夜，google 探索了 n 个问题，熬掉了十几根头发，最终发现解决方案就在眼前，让人可恨又可爱的 man 手册。终究还是对搜索引擎的过度依赖导致浪费了过多的时间在这个问题上面，不过也还不错，有比较巨大的收获。记录一下这次历程，深刻学习教训。
  起初目的是搭建 libvirt + webvirtmgr 环境，因此需要开启 libvirt tcp 监听，操作系统选择的 ubuntu20.04，在网上搜索的基本都是低于20.04 版本的教程，也因此走了很多弯路

## 环境
---
### 系统以及版本 （只列出必备）
- ubuntu20.04
- libvirt-daemon-system (版本  libvirt-daemon_6.0.0-0ubuntu8_amd64)

  网上大部分教程都不能盲目使用，需要根据操作系统和版本酌情参考。本次安装为纯净 ubuntu20.04 环境安装（重装了好几次系统）。
## libvirt 依赖安装以及配置
### 安装必备包
```shell
sudo apt-get install libvirt-daemon-system libvirt-clients
sudo apt-get install sasl2-bin libsasl2-modules bridge-utils
```
### libvirtd 修改配置项目
#### 修改启动项参数
  默认安装之后，libvirtd 默认监听 unix 域套接字，需要开启 tcp 监听。打开 libvirtd 启动项修改配置。
**sudo vim /etc/default/libvirtd**

```shell
libvirtd_opts="-l"
```
#### 修改配置文件
修改  **/etc/libvirt/libvirtd.conf** ,修改如下配置
```shell
# 允许tcp监听
listen_tcp = 1
listen_tls = 0

# 配置tcp通过sasl认证
auth_tcp = "none"
```

### 屏蔽其他无关服务
```shell
sudo systemctl mask libvirtd.socket libvirtd-ro.socket \
libvirtd-admin.socket libvirtd-tls.socket libvirtd-tcp.socket
```
重启服务即可

### 服务相关命令
```shell
# 重启服务
sudo systemctl restart libvirtd
# 启动服务
sudo systemctl start libvirtd
# 查看服务状态
sudo systemctl status libvirtd
# 停止服务
sudo systemctl stop libvirtd
```

### 测试服务是否启动成功

```shell
virsh -c qemu+tcp://localhost/system list
```




### 其他命令
```shell
# 取消屏蔽
sudo systemctl unmask libvirtd.socket
```

### 参考资料

[man 手册](https://manpages.ubuntu.com/manpages/focal/man8/libvirtd.8.html)

[nginx](/samba/git_stash_usage)

## 配置网桥

ubuntu 中，使用 netplan 配置网络，修改 **01-network-manager-all.yaml**
```shell
sudo vim /etc/netplan/01-network-manager-all.yaml
```
```shell
# Let NetworkManager manage all devices on this system
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: false
      dhcp6: false

  bridges:
    br0:
      addresses:
        - 192.168.0.109/24
      gateway4: 192.168.0.1
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114, 192.168.0.1]
      interfaces:
        - enp1s0
```
上面 ip 地址，物理网卡名称和虚拟网卡名称根据实际情况更改,改完之后，执行
```shell
sudo netplan apply
```
更新网络配置，执行
```shell
sudo brcth show
```
查看网络是否正常，br0网卡是否活跃

## 安装 WebVirtMgr
### 安装 python2 python-pip
Ubuntu 20.04 不再包含 python2 的 pip 源。可以通过 Python2 执行 get-pip.py 脚本安装。
启用 universe 存储库
```shell
sudo add-apt-repository universe
```
安装 python 2
```shell
sudo apt update
sudo apt install python2
```
用 curl 下载 get-pip.py 脚本
```shell
# 操作系统中如果没有 curl, 先下载 curl
sudo apt intall curl
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
```
安装 pip
```shell
sudo python2 get-pip.py
# 查看是否安装成功
pip2 --version
```
输出
```shell
pip 20.3.4 from /home/quan/.local/lib/python2.7/site-packages/pip (python 2.7)
```
安装 python-libvirt







