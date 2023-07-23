---
title: gcc编译器
date: 2019-11-09 13:57
tags:
- C/C++
---
* -o
编译输出可执行文件
```
gcc hello.c -o helloc

```
* -I
指定头文件位置

```
# 指定头文件位置在当前目录下的inc目录中
gcc -I ./inc hello.c -o hello.out
```
<!--more-->
* -E
预编译

```
#输出经过预处理之后的代码,包括宏替换，include文件
gcc -E hello.c -o hello.E

```
* -g
 生成gdb可调试程序

```
gcc -g hello.c

```
* -Wall 
显示所有警告信息

### g++选项
* -L
表示编译程序按照-L指定的路径搜索路径，在-L后面可以一次用-l指定多个库。
* -l
表示库的名字，库的名字通常是lib+x+.so.x，指定库为x
例如`libz.so.1`只需要写-lz
