---
title: jupyter notebook常用功能
date: 2020-07-14 19:02:10
tags: jupyter notebook
---

## jupyter notebook常用功能

jupyter notebook端口为8888,可以在浏览器中输入*localhost:8888*访问
### 1. 转化ipynb成py文件

`jupyter nbconvert --to script fashion_mnist.ipynb`

使用该命令后在同级目录生成前缀名相同的py文件

### 2. 运行所有cell中的代码

Options：单元格>运行所有单元格

### 3. 启动jupyter notebbook

在命令行中输入`jupyter notebook`，即在该目录下，打开一个notebook
### 4. 在jupyter两种模式切换
由命令模式转换为编辑模式:按键*ENTER*
由编辑模式到命令模式:按键*ESC* 
### 5. markdown和code模式切换
code->markdown: 命令模式下按键*m*进入
markdown->code: 按键*y*进入

