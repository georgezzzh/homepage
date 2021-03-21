---
title: nginx配置https导致nginx服务启动失败
date: 2019-09-26 09:12:14
tags:
- nginx
---
首先要检测一下是否,tomcat配置过https，如果tomcat配置过https，监听443端口，则nginx无法占用443端口，所以在$tomct/conf/server.xml中将redirect的443全部
修改为8443，同时将443端口注释掉，恢复成tomcat默认的配置
