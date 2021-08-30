---
title: "git stash 命令 储藏修改"
date: 2021-05-18T03:41:23Z
draft: false
---

# git stash 命令 储藏修改
---
## 问题
在新增业务的过程中，需要修复一个紧急bug，但是当前分支已经修改了很多内容，想要切换分支，但是不想提交当前的内容。
## 解决办法
可以把当前的修改内容储藏起来，并将储藏内容推送到栈上
## 用法
### 保存
```shell
git stash [save message]
```
save为可选项，可对当前的储藏添加备注信息。
### 取出 
```shell
git stash pop			#  恢复最近一次的储藏内容，恢复一次即删除
git stash pop stash@{num}	# 恢复指定序号的储藏内容
git stash apply			# 同上，不过可恢复多次
git stash apply stash@{num}	# 同上
```
### 其它
```shell
git stash list
git stash drop 
git stash clear
```




