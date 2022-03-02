---
title: c strtok 字符串分割
date: 2021-05-07 12:47:32
categories: [c]
tags: [strtok]
draft: false
---

# c strtok 字符串分割
---
## 常规写法，线程不安全
```c
#include <string.h>

void str_split(char *str, char * delim)
{
    char *ptr =  strtok(str, delim);
    while (ptr != NULL) {
        printf("%s\n", ptr);
        ptr = strtok(NULL, delim);
    }

    return;
}
```

## 线程安全
```c
#include <string.h>
void str_split_safe(char *str, char *delim)
{
    char strtokbuff[256] = { 0 };
    char *ptr = strtok_r(str, delim, &strtokbuff);
    while (ptr != NULL) {
        printf("%s\n", ptr);
        ptr= strtok_r(NULL, delim, &strtokbuff);
    }
}
```