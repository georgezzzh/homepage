---
title: gdb调试
date: 2019-09-25 21:43:41
tags:
- C/C++
- gdb
---

1. 编译时`gcc -g main.c -o main`
加上g选项，生成目标文件才能用gdb调试    
2. `objdump -dS main`,反汇编，将汇编代码和C代码对应起来
3. 启动调试`gdb main`
4. 列出原来的代码 `list`,通过空格翻页
5. 打断点，`b 10`，在第10行打断点，指的是，程序仅会运行到第10行(10行未运行)
6. 运行，`run(r)`
7. 查看变量, `print(a)`，查看a的变量
<!--more-->
2. 设置命令行参数,`set args 命令行参数`,然后再调试运行
3. 单步调试`s(step)`
4. 下一步`n(next)`
5. 把函数执行完`finish`
6. 观察变量`i(info) local`
7. 查看栈帧 `f(frame)`
8. 选择栈帧` f 1`, 选择第一栈帧
9. 打印变量
`p(print)`
10. 跟踪变量
`display sum`
//每次都显示sum的值
11. 取消跟踪
`undisplay`      
12. 插入断点 `b(break) 7`
意思是在第七行插入断点, break命令的参数也可以是函数名，表示在某一函数开头设置断点，现在用c连续运行而非单步运行，程序到达断点会自动停下来。
13. 查看断点`info breakpoints`
14. 删除断点`delete breakpoints 1`,删除断点1
15. 禁用断点`disable breakpoints 1`,禁用断点1
16. 重新开始调试`run`,重新调试
17. 观察点`watch input[4]`,启用观察点，观察input[4]这个值的变化，当出现变化时，会打印出old value和new value
### 跟踪段错误的位置
如果出现”段错误“，会在程序同级目录生成一个core文件，但是Ubuntu不会生成。core文件主要用来调试
在ubuntu中执行如下命令`ulimit -cunlimited`，允许生成core文件
* 运行  ./main.out 
查看同级目录出现core文件
* gdb进行调试 `gdb main.out core`
会定位出，在哪一行出现段错误
