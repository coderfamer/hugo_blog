---
title: k8s 安装 dashboard
date: 2022-06-28 15:07:32
categories: ['software', 'harbor']
tags: ['harbor', 'k8s']
draft: false

---

# k8s 安装 dashboard

---

Kubernetes 的 Web UI 网页管理工具是 kubernetes-dashboard，可提供部署应用、资源对象管理、系统日志查询、系统监控等常用的集群管理功能。为了在页面上显示系统资源的使用情况，需要部署 Meterics Server。

## 安装教程

### 准备

- kubernetes-dashboard 一键部署 yaml
- dashboard 镜像
- metrics-scraper 镜像

yaml 下载

```shell
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
```

镜像下载

```shell
docker pull kubernetesui/metrics-scraper:v1.0.8
docker save -o metrics-scraper.tar kubernetesui/metrics-scraper:v1.0.8
docker pull kubernetesui/dashboard:v2.6.0
docker save -o dashboard.tar kubernetesui/dashboard:v2.6.0
```

保存这些文件并且复制到待安装机器

![](E:\hugo_blog\images\software\k8s-03_dashboard_list.png)

### 安装

加载镜像文件

```shell
docker load --input dashboard.tar
docker load --input metrics-scrapter.tar
```

修改 recommend.yaml 配置项

```shell
sepc:
  type: NodePort
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 30008
```

