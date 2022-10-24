---
title: git 基于某个分支重新构建项目
date: 2022-03-08 15:07:32
categories: ['']
tags: ['git']
draft: false


---

# git 基于某个分支重建项目

##  简介

项目中因为不同需求构建多分支，其中某个分支改动量较大，想要独立成一个业务项目，本文记录了相关操作

## 过程

### 重建

在项目目录中检出分支

```shell
git checkout <branch_name>
```

拷贝项目到一个新的目录，并切换目录

```shell
cp  project new_project -r && cd new_project
```

替换当前分支为主分支

```shell
git branch -D master
git branch -m master
```

建立远程仓库（根据项目仓库所在位置，自行创建）

修改仓库地址并提交内容

```shell
git remote set-url origin <new-remote-url>
git push origin master
```

### 清理分支

如果旧项目中有很多分支和远程分支，在新项目中会显的杂乱，可以利用 git 命令清理一些无效分支，即不想保存在 remote 的分支

查看无效分支

```shell
git remote prune origin --dry-run
```

清理无效分支

```shell
git remote prune origin
```

如果本地还有一些已检出的分支需要清理，需要手动在本地清理

```shell
git branch -d <branch1 branch2 ...>
```

