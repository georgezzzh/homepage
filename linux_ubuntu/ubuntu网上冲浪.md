---
title:  ubuntu下用锐捷认证 & SSR & SS & WIFI
date: 2019-10-26 17:11:17
tags:
- Linux
- Ubuntu
---
### ubuntu下用锐捷认证校园网
1. 下载mentohust

   [下载地址](https://code.google.com/archive/p/mentohust/downloads)  
   [github另存档地址](https://github.com/georgezzzh/resource/raw/master/linux/%E9%94%90%E6%8D%B7ubuntu.tar)

   ![Ubuntu选择deb](https://github.com/georgezzzh/resource/blob/master/linux/ruijiedownload.png?raw=true)

2. 安装 

3. 运行

```shell
# -u表示用户名 -p表示密码 -d1表示DHCP动态选择IP地址 v表示版本号,scu为6.82 
# &表示后台运行，否则窗口关闭认证就退出了
sudo mentohust -u20161414xxxx -p123456 -d1 -v6.82 &

```

4. 出现的问题

>0.提示“不允许使用的客户端类型”：学校禁用了xrgsu，使用-v参数指定版本号
>
>1.提示“客户端版本过低
>
>复制相关文件（"8021x.exe"和"W32N55.dll"）到/etc/mentohust/
>
>8021x.exe和W32n55.dll文件在Windows的锐捷认证软件(四川大学认证客户端)下有相关文件

5. 其他注意事项

* mentohust命令参数-n 是网卡名，查看网卡时，使用`ifconfig`会给出网卡的名字，例如这里给的是*enp2s0*
![ifconfig](https://github.com/georgezzzh/resource/raw/master/linux/ifconfig.png)
* mentohust配置文件保存在`/etc/mentohust.conf`,进行编辑修改

6. 写成简单的脚本
```shell
if [ "$1" = "-help" -o "$1" = "--help" ]; then
				echo "-r restart"
elif [ "$1" = "-r" ] ; then
				echo "重启网络连接"
				pkill -f mentohust
				sudo mentohust &
elif [ -z "$1" ]; then
	echo "启动网络连接"
	sudo pkill -f mentohust
	sudo mentohust -u2020xxxxxx -p123456 -d1 -v6.82 &
else
				echo "未定义选项"
fi
#d1表示动态分配dhcp v后面是版本号
#sudo mentohust -u2016xxxxx -p123456 -d1 -v6.82 
```

### ubuntu下用SSR

1. 下载electron-ssr

   [下载地址](https://github.com/qingshuisiyuan/electron-ssr-backup/releases)

   选择deb版本就可以

2. 下载打开填写IP,端口等参数就ok

### ubuntu中使用shadowsocks

1. 在github中找一下shadowsocks-qt5，这个仓库，找到下载链接时，下载下来,是一个AppImage的文件
2. 在GUI环境下填好配置参数
**Ubuntu下，没有第三步的化，即使在ss-qt5软件中测试延迟成功，也无法访问google**
3. 在ubuntu系统设置->网络->网络代理中修改为：手动代理
![](https://github.com/georgezzzh/resource/blob/master/linux/ubuntu18_set.png?raw=true)
![](https://github.com/georgezzzh/resource/blob/master/linux/ubuntu18_NetSet.png?raw=true)
4. 下载一个omegaProxy插件，设置为系统代理
5. FireFox没有成功，不明所以,原因应该是**常规>网络设置**
### ubuntu下用v2ray
1. 首先下载Qv2ray的release版本，内容为AppImage
2. 下载v2ray core的release版本，并在Qv2ray中填写
![首选项中的配置](https://github.com/georgezzzh/resource/raw/master/linux/v2rayCore.png)

### ubuntu下台式机用WIFI

淘宝搜索"Linux笔记本ubuntu台式机wifi接收器manjaro免驱动usb无线网卡小",产品型号是COMFAST,CF-WU810N,该网卡Linux免驱动，TP-Link的无线网卡装驱动太过于麻烦，其中买了一个网卡，发现官方支持内核到4.x，5.0的内核无法编译成功。。。
