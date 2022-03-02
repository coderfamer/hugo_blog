---
title: git describe 命令详解
date: 2021-08-29 15:07:32
categories: [git]
tags: [git describe]
draft: false

---

# git describe 命令详解

---

## git 获取最新的 tag

---

查找最近的tag

```shell
git describe
# 不需要后缀
git describe --abbrev=0
```

获取当前分支的tag

```shell
git describe --abbrev=0 --tags
```

获取所有分支的tag

```shell
git describe --tags `git rev-list --tags --max-count=1`
```



