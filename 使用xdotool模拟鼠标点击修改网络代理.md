---
title: 使用xdotool模拟鼠标点击修改网络代理
date: 2020-10-13 23:28:18
tags:
- Ubuntu
---

```shell
# 打开gnome-settings
gnome-control-center &
# 模拟鼠标移动 x y
xdotool mousemove 1337 632
#模拟鼠标左键点击
xdotool click 1
# 判断关闭还是开启
if [ "$1" = "-k" ];then
	echo 关闭代理
	xdotool mousemove 805 474
else
	echo 开启代理
	xdotool mousemove 805 438
fi
sleep 0.5
xdotool click 1
xdotool mousemove 1157 359
xdotool click 1
xdotool mousemove 20 20

```
