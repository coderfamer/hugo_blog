---
title: git describe 命令详解
date: 2021-08-29 15:07:32
categories: [git]
tags: [git prune]
draft: false

---

# git prune 清理无效分支

---
 git 在远程目录上删除了一个分支，本地追踪分支并不会删除，命令 **git prune** 可以清理无效分支

```shell
# 查看失效分支
git remote prune origin --dry-run
# 清理失效分支
git remove prune origin
```


