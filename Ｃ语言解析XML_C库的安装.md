---
title: C/C++语言解析XML
date: 2019-10-31 21:47:43
tags:
- C/C++
- Ｃ库的使用
---
### C语言解析XML

解析xml, C语言有libxml2库可以用. [官网地址](http://xmlsoft.org/downloads.html), 然后点击FTP链接[FTP链接]([ftp://xmlsoft.org/libxml2/](ftp://xmlsoft.org/libxml2/))就可以下载了. 

我这里是找了一个libxml2-2.9.9.tar.gz下载
<!--more-->
### 安装

```shell
./configure
make
# 注意有些命令执行之后失败, 可能需要提权限,因为要复制文件到/usr/local/include, /usr/local/lib
sudo make install
```

安装成功切换到/usr/local/include目录, 发现有个**libxml2**目录, 然后在CLion中建立C项目, 发现`#include <libxml2/tree.h>`时,提示找不到. 

* 有两个解决方法: 

  1. ```shell
     $: pwd
     $: /usr/local/include/libxml2
     # 将/usr/local/include/libxml2中的libxml 目录全部复制到usr/local/inlcude下面
     # 之后写程序只用包含# include<libxml/tree.h>就可以了
     $: cp -r libxml ..
     
     ```

  2. 直接包括头文件

     ```c
     //这种方式也可以
     #include<libxml2/libxml/tree.h>
     ```

### 新建C工程

我这里在ubuntu平台安装的CLion进行新建C语言项目

直接加入头文件之后, 写入`xmlReadFile()`等函数无提示, 运行时报错. 

* 解决措施: 要在CMakeLists.txt进行链接: 

  主要是进行链接*link_libraries(libxml2.so)*, Visual Studio平台可以查找如何进行链接

  ```cmake
  cmake_minimum_required(VERSION 3.14)
  project(XML C)
  # 链接库, 不连接会报错
  link_libraries(libxml2.so)
  set(CMAKE_C_STANDARD 99)
  add_executable(XML main.c)
  ```

### 万事俱备,开始写代码

test.xml

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<bookstore>
    <book>
        <title lang="eng">Harry Potter</title>
        <price>29.99</price>
    </book>
    <book>
        <title lang="eng">Learning XML</title>
        <price>39.95</price>
    </book>
</bookstore>
```

main.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <libxml2/libxml/tree.h>
#include <libxml2/libxml/parser.h>
int main() {

    xmlDocPtr doc=NULL;
    xmlNodePtr root=NULL,cur=NULL;
    xmlKeepBlanksDefault(0);
    const char* doc_name="/home/george/Documents/CLionProjects/XML/test.xml";
    doc=xmlReadFile(doc_name,"UTF-8",XML_PARSE_RECOVER);
    if(doc==NULL){
        fprintf(stderr,"error!can't open the file");
        exit(-1);
    }
    root=xmlDocGetRootElement(doc);
    if(root==NULL){
        fprintf(stderr,"file is empty");
        exit(-1);
    }
    cur=root->children;
    while(cur!=NULL){
        //BAD_CAST是一个宏,转换char到xmlChar,相当于(xmlChar*)
        //寻找等于<book>的标签,xmlStrcmp()==0意味着匹配,这里与strcmp()类似的返回值
        if(0==xmlStrcmp(cur->name,BAD_CAST("book"))){
            xmlNodePtr nptr= cur->children;
            while(nptr!=NULL){
                if(0==xmlStrcmp(nptr->name,BAD_CAST("title"))){
                    printf("title:%s\n",(char*)XML_GET_CONTENT(nptr->children));
                }else if(0==xmlStrcmp(nptr->name,BAD_CAST("price"))){
                    printf("price:%s\n",(char*)XML_GET_CONTENT(nptr->children));
                }
                nptr=nptr->next;
            }
        }
       cur=cur->next;
    }
    xmlFreeDoc(doc);
    xmlCleanupParser();
    xmlMemoryDump();
    return 0;
}
```

### 文档示例

可以将github中这个项目clone下来, 找docs看一下

[博客参考](https://www.cnblogs.com/catgatp/p/6505451.html)
