---
title: ubuntu20.04安装谷歌拼音
date: 2020-06-16 13:48:52
tags: Ubuntu
---
因为ubuntu20.04默认的智能拼音输入法与WPS-Office冲突，导致wps启动奇慢无比，所以重新配置fcitx输入法，系统默认是ibus输入法。
### 1. 谷歌输入法安装
```shell
sudo apt-get install fcitx fcitx-googlepinyin im-config
```
### 2. 启动Fcitx设置输入法
命令行运行,一路点击下一步
```shell
im-config
```
### 3.重启系统
顶部状态栏出现谷歌输入法的选项
