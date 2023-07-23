---
title: mysql8.0.20在ubuntu20中的安装
date: 2020-07-24 17:53:22
tags:
- mysql 
- Linux
---

1. `apt-get install mysql-client mysql-server`
2. `cat /etc/mysql/debian.cnf`
3. 查找到debian-sys-maint的密码，以这个密码进入mysql
4. `mysql -udebian-sys-maint -p`
5. 输入刚才第三步查找到的密码
6. `use mysql;`
7. `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';`
8. 退出mysql重进
9. 可以直接用`mysql -u root `输入上一步设置的密码进入
<!--more-->

---
注意不要删除`/var/log/mysql`，否则导致mysql启动不了
#### 重新安装之前进行彻底卸载mysql

```shells
# 删除所有库
sudo rm /var/lib/mysql -R
# 删除配置文件
sudo rm /etc/mysql -R
sudo apt-get autoremove mysql* --purge 
```
