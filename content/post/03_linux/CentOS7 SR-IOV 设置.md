---
title: CentOS7 SR-IOV 设置
date: 2022-02-21 12:07:32
categories: ['']
tags: ['sr-iov', '虚拟化']
draft:  false
---

---

## 简介
SR-IOV（Single Root I/O Virtualization） 是由 PIC-SIG 组织定义的 PCIe 规范的扩展规范，目的是通过一种标准规范，为虚拟机提供独立的内存空间、终端、DMA数据流。

## 配置过程

## bios 设置 

配置  **VT-d** 和 **SR-IOV** 开关

### 开启iommu
修改 grub 文件
```shell
vim /etc/default/grub
```

在 GRUB_CMDLINE_LINUX 行尾，追加上 **inter_iommu=on iommu=pt**

```shell
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet intel_iommu=on iommu=pt"
```

重新生成启动配置

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg
```

### 启动时生成虚拟网卡

vim /etc/rc.local，在文件尾部添加如下内容

```shell
echo 2 > /sys/class/net/<ethname>/device/sriov_numvfs # 2: 要生成的虚拟网卡个数 <ethname>:生成虚拟网卡的物理网卡名
```

虚拟网卡添加固定mac地址

```shell
ip link set <ethname> vf 0 mac 00:11:22:33:44:55
ip link set <ethname> vf 1 mac 00:11:22:33:44:56
```



rc.local添加可执行权限

```shell
chmod +x /etc/rc.local
```

重启

```shell
reboot
```

查看网卡信息

```shell
lspci | grep 'Virtual Function'
```

## 其他

查看网卡

```shell
lspci | grep Eth  # 查看所有网卡
ethtool <ethname> # 查看某个网卡信息
```

重新生成内核启动项

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg
```

## 问题

[网卡无法生成vf，intel/mellanox，write error: Cannot allocate memory “not enough MMIO resources for SR-IOV” (icode9.com)](https://www.icode9.com/content-4-1045594.html)

[(53条消息) echo: write error: Cannot allocate memory_u013431916的博客-CSDN博客](https://blog.csdn.net/u013431916/article/details/103245758)

## 引用

[CentOS7でSR-IOV設定 - Metonymical Deflection (hatenablog.com)](https://metonymical.hatenablog.com/entry/2018/04/11/193408)

[Configure SR-IOV Network Virtual Functions in Linux* KVM* (intel.com)](https://www.intel.com/content/www/us/en/developer/articles/technical/configure-sr-iov-network-virtual-functions-in-linux-kvm.html)

[Chapter 9. SR-IOV support for virtual networking Red Hat Enterprise Linux OpenStack Platform 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_openstack_platform/7/html/networking_guide/sr-iov-support-for-virtual-networking)

[SR-IOV是什么？性能能好到什么程度？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/91197211)