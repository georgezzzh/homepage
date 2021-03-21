---
title: Postgresql基本使用
date: 2020-10-26 23:14:34
---

*未加说明，一切操作在linux下(ubuntu20.04)，正文中引用标志一般是为了说明运行命令后终端输出的内容*

*tips: 在postgresql shell中运行所有命令需要加上 **;** 号，才可以执行成功，否则直接回车，会把所有输入内容拼接起来直到有**;** 后一起执行*

### 1. 安装

```shell
sudo apt install postgresql postgresql-contrib
```
<!--more-->
### 2.  在命令行中切换到`postgres`

安装数据库之后，会为linux系统创建一个postgres的user

```shell
sudo su postgres
```

> postgres@george-ubuntu20:/home/george$ 

### 3.  打开PostgreSQL  shell

```shell
psql
```

> psql (12.4 (Ubuntu 12.4-0ubuntu0.20.04.1))
> Type "help" for help.
>
> postgres=# 

### 4. 创建一个新用户

```shell
# 创建用户 george
create user george with password 'yourpassword';
# 添加超级权限
alter user george with superuser;
# 删除用户
drop user george;
```

### 5. 创建数据库

```shell
# 创建一个toydb数据库,它的所有者是postgres，如果不指定owner，默认owner为当前登录的用户
create database toydb owner postgres;
# 删除数据库
drop database toydb;
```

---

预置内容处理完成，接下来正常使用

### 1. 直接连接数据库

```shell
 # 在shell终端，默认用户就可以，不用在postgres用户下
 # d是数据库名字，是之前创建的，-U指定用户名
 psql -U george -d university
```

> george@george-ubuntu20:~$ psql -U george -d university
> psql (12.4 (Ubuntu 12.4-0ubuntu0.20.04.1))
> Type "help" for help.
>
> university=# 

1. 显示所有用户`\du`

2. 显示所有数据库 `\l`

3. 显示所有表`\dt`或者`\d`

4. 显示表的结构 `\d tablename`

   例如显示`person`表结构，可以用`\d person`来查看
   
5. 切换数据库 `\c databasename`

   >You are now connected to database "toydb" as user "george".
   >toydb-# 

---

### 0. 从SQL文件中读取数据

进入数据库之后直接执行

`\i  ~/Downloads/DDL.sql`

### 1. 其他增删改查与其他关系型数据库类似

为了上课使用，下载下来看一下，支持的SQL标准更多一些。

---

引用网页

[linux-cn关于Postgresql](https://linux.cn/article-11480-1.html)

知乎及其他网站
