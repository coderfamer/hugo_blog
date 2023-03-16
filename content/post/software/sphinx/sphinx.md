---
title: "Sphinx 安装使用"
date: 2022-12-07T17:50:55+08:00
categories:
  - software
tags:
  - sphinx
draft: false
---

---
sphinx 文档构建系统
## 简介
## 安装
为了防止环境混乱，建议使用 conda 创建新环境安装
### 依赖
 - conda
### 安装 conda  并创建新环境
#### 安装 conda
#### 创建新虚拟环境
```shell
conda env create -n sphinx --no-default-packages --file=req.txt
conda activate sphinx
```
**cat req.txt**
```shell
python=3.8
pip
```
conda 会创建一个只包含 python 和 pip 名为 **sphinx** 的环境
#### 安装 sphinx 以及 依赖
安装 sphinx
```shell
pip install sphinx
```
若需要支持 markdown 和表格，安装
```shell
pip install recommonmark 
pip install sphinx-markdown-tables
```
若需要生成 pdf
```shell
pip install rst2pdf
```

## 使用教程
使用 **sphinx-quickstart** 创建空白项目
```shell
sphinx-quickstart
```
配置相关属性，构建
```shell
make html 
```
build/html 目录会生成静态网页


sphinx-quickstart
sphinx-build

## 引用
[sphinx-practise.readthedocs.io](https://sphinx-practise.readthedocs.io/zh_CN/latest/index.html)

