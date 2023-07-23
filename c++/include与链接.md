---
title: include与链接库
date: 2019-09-25 21:45:22
tags:
- C/C++
---

### `include <example.h>` 
对于用尖括号括起来的头文件，gcc首先查找-I 选项指定的目录，然后查找系统的头文件目录(通常是/usr/include)；  
### `include "example.h"`
对于用引号包括的头文件，gcc首先查找包含头文件.c所在的目录，然后查找-I选项制定的目录，然后查找系统的头文件目录。
<!--more-->
### 编译不在同一个目录下的.c文件
```
george@machine:~/Public/temp$ tree
.
├── main.c
└── stack
    ├── stack.c
    └── stack.h

```
进行编译  
`gcc main.c stack/stack.c -I stack -o main`  
-I的含义是gcc头文件要在stack文件夹中找
### 链接详解
Linux系统中动态链接库在`/lib/x86_64-linux-gnu`下，pthread动态库在该目录，自己安装的库一般在`/usr/local/lib`中,例如安装的boost和libxml动态库都位于此。

