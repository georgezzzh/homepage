---
title: ubuntu中一些好用的软件
date: 2019-09-26 17:07:55
tags:
- Ubuntu
- Linux
---
### 自带软件

#### 自带的remmina，SSH客户端
自带这款SSH软件足够好用了，支持FTP和SSH，相当于Xshell和xFTP。也有保存密码的功能。
#### 系统监视器
自带软件，可以看CPU/内存/网络的使用信息
#### 磁盘分析工具baobab
该工具缺点是存在权限问题，所以启动时用`sudo baobab`就ok了。
<!--more-->
#### 卸载一些软件
```shell
# 卸载计算器，有命令行bc，没必要用这个
sudo apt-get remove --purge gnome-calculator

```
### 其他软件
#### 有道词典命令行

```shell
# 依赖nodejs环境
sudo apt install nodejs
sudo apt install npm
# global config, install command line dict
sudo npm install yddict -g
```
#### 截图软件shutter|Flameshot
设置快捷键cltr+alt+A启动shutter，可以和windows中用QQ的习惯相似。
#### 录屏软件Kazam
简单实用
#### Nomacs图片查看器
自带的图片查看器，每次都得重新导入图片，不方便，Nomacs有显示图片的元数据和缩略图，也不用每次重新导入图片
#### GitHub Desktop 
第三方软件，用electron做的，效果很不错。与windows下官方出品基本无差别.
https://github.com/shiftkey/desktop
#### Spotify|网易云音乐
Spotify在Ubuntu自带的应用商店安装即可。
听歌软件
#### VLC视频播放器
比自带的要强大，大多数视频都可以解码
#### WPS全家桶
LiberOffice太烂了，Linux下的WPS无广告，基本满足要求。
#### 微软OneDrive
开源第三方实现，详细教程`https://zhuanlan.zhihu.com/p/343293386`

github项目地址：`https://github.com/abraunegg/onedrive`

```
sudo add-apt-repository ppa:yann1ck/onedrive
sudo apt-get update
sudo apt-get install onedrive
```

