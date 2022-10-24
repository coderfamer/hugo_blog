---
title:  CentOS 7 安装 harbor
date: 2022-06-06 15:07:32
categories: ['software', 'harbor']
tags: ['harbor', 'k8s']
draft: false

---

# CentOS 7 安装 harbor

---

## 简介

harbor 镜像仓库是由 VMware 开源的一款企业级镜像仓库，主要用于存放 docker 的镜像

## 安装

### 系统要求

- docker 17.06.0-ce+
- docker-compose 1.18.0+

### 安装 dockers-compose

下载 [docker-compose](https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64) 并安装

```shell
wget -O - https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64 > docker-compose
mv docker-compose /usr/local/bin/docker-compose-2.6.0
ln -fs /usr/local/bin/docker-compose-2.6.0 /usr/bin/docker-compose
```

验证是否安装成功

```shell
docker-compose --version 
```

输出类似 

**Docker Compose version v2.6.0**

说明安装成功

### 安装 harbor

下载 [harbor](https://github.com/goharbor/harbor/releases/download/v2.5.1/harbor-offline-installer-v2.5.1.tgz) 离线版本

```shell
wget https://github.com/goharbor/harbor/releases/download/v2.5.1/harbor-offline-installer-v2.5.1.tgz
tar -zxvf harbor-offline-installer-v2.5.1.tgz && cd harbor
cp harbor.yml.tmpl harbor.yml  # 复制 harbor 配置文件模板
```

#### 修改 harbor.yml 配置文件

修改 hostname 和 端口，如若想其他主机访问 harbor ，不建议修改 hostname 为 127.0.0.1 或者 localhost

```shell
hostname : 192.168.105.110

http:
  port: 8090
```



配置 harbor 网络访问协议，默认为 https，使用 http 需要注释 https模块

```shell
# https:
#  # https port for harbor, default is 443
#  port: 443
#  # The paht of cert and key files for nginx
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path
```

**安装 harbor**

```shell
./install
```



![](E:\hugo_blog\images\software\k8s-02_harbor_install.png)

如图说明安装成功

## 访问

#### 访问 harbor web 页面

http://192.168.105.110:8090   输入账户名默认密码 admin/Harbor12345

![](E:\hugo_blog\images\software\k8s-02_harbor_web.png)

### CentOS 访问 harbor 服务器

centos 访问 harbor 服务器需要 docker 配置私有云地址，打开 **/etc/docker/daemon.json** 文件配置，添加如下内容

```shell
"insecure-registries" : ["192.168.0.100:8090"]  # ip:端口号
```

docker 登录 harbor

```shell
docker login 192.168.0.100:8090  # 输入用户名 密码即可
```



## 推送、拉取

## 引用







