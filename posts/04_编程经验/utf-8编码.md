---
title: utf-8 编码
date: 2022-03-02 15:07:32
categories: ['编程经验']
tags: ['utf-8']
draft: false

---

# utf-8 编码

## 简介

UTF-8 （8-bit Unicode Transformation Format）是一种针对**Unicode**的可变长度字符编码，也是一种前缀码。它可以用一至四个字节对**Unicode**字符集中的所有有效编码点进行编码，属于**Unicode**标准的一部分，最初由肯·汤普逊和罗布·派克提出。

## 编码方式

| range        | Byte1    | Byte2    | Byte3    | Byte4    |
| ------------ | -------- | -------- | -------- | -------- |
| 0000-007F    | 0xxxxxxx |          |          |          |
| 0080-07FF    | 110xxxxx | 10xxxxxx |          |          |
| 0800-FFFF    | 1110xxxx | 10xxxxxx | 10xxxxxx |          |
| 10000-10FFFF | 11110xxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |

对于UTF-8字符：

0xxxxxxx：单字节表示一个字符

110xxxxx： 双字节表示一个字符

1110xxxxx： 三字节表示一个字符

11110xxx： 四字节表示一个字符

## 判定标准

假设字符串为p(c语言)

```c
// 双字节
(p[0] & 0xE0) == 0xC0 && (p[1] & 0xC0) == 0x80
// 三字节
(p[0] & 0xF0) == 0xE0 && (p[1] & 0xC0) == 0x80 && (p[2] & 0xC0) == 0x80
// 四字节
(p[0] & 0xF8) == 0xF0 && (p[1] & 0xC0) == 0x80 && (p[2] & 0xC0) == 0x80 && (p[3] & 0xC0) == 0x80
```

## 引用

[UTF-8 strings in C (1/3) - DEV Community](https://dev.to/rdentato/utf-8-strings-in-c-1-3-42a4)

[UTF-8 - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/UTF-8)