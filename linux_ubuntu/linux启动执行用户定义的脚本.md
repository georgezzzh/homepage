---
title: systemd管理开机自启服务
date: 2020-10-30 23:02:43
tags:
- Linux
---
### linux启动执行用户定义的服务

在这个目录下，创建一个rj.service

```shell
/etc/systemd/system
```

### 内容

```shell
[Unit]
Description=ruijie service
After=network.target
Wants=network.target

[Service]
Type=simple
#ExecStart=/usr/bin/mentohust
ExecStart=/bin/bash /home/george/ruijie.sh
User=root
Restart=on-failure
# 设置启动时长限制为20s
TimeoutStartSec=30
# StartLimitInterval=30min
# 配置错误不要重启
#RestartPreventExitStatus=23
[Install]
WantedBy=multi-user.target
```
>

### enable服务

```shell
#使rj.service启用
systemctl enable rj.service
#禁用
systemctl disable rj.service
```

### 重启即可加载

```shell
# 使修改过的service刷新
systemctl daemon-reload
# 开机自启动rj.service，这一步不执行的话，开机是自启动不了的
systemctl enable rj.service
```

### 启动服务

注意一下，在开机重启测试之前，要先检查一下，服务是否能运行成功，`systemctl status rj`，查看是否状态为running。

*值得注意一点，在ruijie.sh脚本中不要使用&来进行后台运行，否则运行失败，去掉后台运行，systemd会默认守护。*

