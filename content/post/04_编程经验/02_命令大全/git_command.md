---
title: "git 常用命令"
date: 2022-12-12T11:11:30+08:00
categories:
  - 命令大全
tags:
  - git 
  - linux
draft: false
---

### git log
``` shell
git log filename
git log -p filename
git log --pretty=oneline filename
git show [commit id] filename 
```



### git stash

```shell
git stash list     			# 查看 stash 列表
git stash show -p  [stashid] # 列出某个 stash 暂存内容
```

### git tag

```shell
git tag -n 
```

### git describe

```shell
git describe --abbrev=0 --tags  # 获取最新的 tag
```

