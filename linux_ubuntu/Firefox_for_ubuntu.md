---
title: Firefox for ubuntu
date: 2020-01-18  12:25
---

1. 浏览器中打开视频网站提示Firefox正在安装组件，以便播放此页面的音频或者视频
解决方法：挂梯子，设置为全局代理

1. 打开火狐->附加组件->插件
2. 选择Widevine内容解密模块
3. 修改允许自动更新(改为默认)

![]( https://www.sonydafa.com/cloud/downloadFile/widewine.png )

2. 打开Firefox无法观看视频
解决方法：安装ffmpeg
```bash
sudo apt install ffmpeg
```
