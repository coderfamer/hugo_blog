---
title: clang-tidy 生成模板文件
date: 2021-03-14 20:07:32
categories: [software, clang]
tags: [clang, tidy]
draft: false

---

```shell
clang-tidy --version # 查看版本
clang-tidy -list-check -checks='*'  # 列出所有可用的检查器 以及简要说明
clang-tidy -dump-config -checks='google-*' > .clang-tidy
```

