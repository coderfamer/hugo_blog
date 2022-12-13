---
title: centos7 clash 科学上网
date: 2022-06-03 15:07:32
categories: ['编程经验']
tags: ['utf-8']
draft: false

---

# centos7 clash 科学上网

---

做开发经常需要访问国外编程网站，下载资料、访问 git 的时候经常会断连，需要部署网络代理，在 windows 上部署非常简单，只要下载 clash 客户端，导入 服务商的代理配置文件即可。在 linux 系统上就需要一些特殊步骤了



## 前提

**软件**

- clash 

- docker

**文件和配置**
clash
  服务商 yaml 配置
  Country.mmdb（启动 clash 会自动下，建议手动下载）

## 安装
---

### clash 客户端
下载  [clash](https://github.com/Dreamacro/clash/releases)

```shell
mkdir /opt/clash && cd /opt/clash
wget https://github.com/Dreamacro/clash/releases/download/v1.10.6/clash-linux-amd64-v1.10.6.gz --no-check-certificate
gunzip -c clash-linux-amd64-v1.10.6.gz > clash && chmod +x clash
```

下载 [Country.mmdb](https://dl.ssrss.club/Country.mmdb)
```shell
mkdir -p ${HOME}/.config/clash && cd ${HOME}/.config/clash
wget https://dl.ssrss.club/Country.mmdb --no-check-certificate
```
导入服务商配置文件
```shell
cd ${HOME}/.config/clash
wget -O config.yaml "class订阅链接"
```

配置代理端口

- **系统用户配置**

```shell
echo export http_proxy=http://127.0.0.1:7890 >> /etc/profile
echo export https_proxy=http://127.0.0.1:7891 >> /etc/profile
source /etc/profile
```

- 当前用户配置
```shell
echo export http_proxy=http://127.0.0.1:7890 >> ${HOME}/.bashrc
echo export https_proxy=http://127.0.0.1:7891 >> ${HOME}/.bashrc
source ${HOME}/.bashrc
```

**config.yaml** 主要配置项修改  **${HOME}/.config/clash**

```shell
# ip 设置为 127.0.0.1 只能本机访问，如若想局域网访问，设置地址为 0.0.0.0
external-controller: 127.0.0.1:9090  # 端口可自行修改
# RESTful  api 口令，后续 web 访问使用，自行设置
secret: "11111"
```



## web 页面配置

有两种方法可以配置 web 页面，分别是 [clash Dashboard]([Clash (razord.top)](http://clash.razord.top/#/proxies)) 和 docker clash Dashbaord

### clash Dashboard 访问

直接访问 **http://clash.razord.top/#/proxies** ，输入 ip、端口和口令，即可进行切换节点、延迟测试等操作

### docker clash DashBoard

确保 docker 已经安装完毕

```shell
docker pull haishanh/yacd
docker run -p 1234:80 -d haishanh/yacd  # 端口映射到 80，随意配置未用端口
```

浏览器输入**主机ip+端口**即可访问 <hostip>:1234












## 引用

[clash核心程序 docker 环境运行，支持 restful接口，dashboard ui，一端运行支持所有的操作系统，Linux|windows|macos|ios|安卓|科学上网 (cfmem.com)](https://www.cfmem.com/2021/09/clash-core-docker.html)

[Linux(Centos7) 使用Clash For Linux网络代理工具教程 – 记忆角落 (199604.com)](https://199604.com/2001)

[Releases · Dreamacro/clash (github.com)](https://github.com/Dreamacro/clash/releases)

[Docker+Clash 部署透明“网关”的实现 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/423684520)

