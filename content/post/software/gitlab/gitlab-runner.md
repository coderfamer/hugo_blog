---
title: gitlab runner 使用配置
date: 2023-02-05 15:07:32
categories: [gitlab]
tags: [gitlab describe]
draft: false

---

## 安装

安装 gitlab runner

```shell
yum install gitlab-runner
```

启动

```shell
gitlab-runner start
```

## 配置

### gitlab 获取 runner 配置信息

gitlab CD/CI 中会生成 gitlab-runner 注册需要的信息。配置分为组配置和项目配置，组配置中的 runner 配置信息为共享配置，当前组下的所有项目都可以使用该配置。项目配置为特殊配置，只有指定项目可以使用该配置。

*settings-->CI/CD-->runners->Expand***

### runner 注册

```shel
[root@localhost ~]# gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=17032 revision=1b659122 version=12.8.0
Running in system-mode.                            
                                                   
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.20.98/
Please enter the gitlab-ci token for this runner:
5yZEuAHLCqFkQNQxWEHg
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: low cpu flags
Please enter the gitlab-ci tags for this runner (comma separated):
test
Registering runner... succeeded                     runner=5yZEuAHL
Please enter the executor: parallels, shell, docker+machine, docker-ssh+machine, kubernetes, custom, docker, docker-ssh, ssh, virtualbox:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 
[root@localhost ~]# 

```

