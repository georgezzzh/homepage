---
title: indent代码格式化工具
author: george
date: 2019-12-04 22:32:02
tags:
- C/C++
---
indent代码格式化工具
* 安装
`sudo apt-get install indent`
* 格式化代码

```shell
#-kr选项是Use Kernighan & Ritchie coding style
# 输出a.c a.c~,a.c~保存原来的代码
indent a.c -kr
```
