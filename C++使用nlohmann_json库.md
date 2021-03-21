---
title: C++使用nlohmann json库
date: 2020-07-27 12:15:48
tags:
- Linux
- C/C++
---

### 1. 安装
[github地址](https://github.com/nlohmann/json)，在这个地址的release中下载hpp文件，即可使用，无需动态链接库

### 2.解析最简单的json文件

有一json文件如下，命名为*file.json*

```json
{
	"pi":3.14,
	"happy":true,
	"name":"trunp"
}
```

在cpp中访问

```cpp
#include <iostream>
#include <fstream>
//这里把下载下来的json.hpp文件移动到/usr/local/include/nlohmann中，这样就可以用include<>搜索了
#include <nlohmann/json.hpp>
using nlohmann::json;
int main()
{
	std::ifstream i("file.json");
	json j;
	i>>j;
    //解析json数据
	double d=j["pi"];
	bool bl = j["happy"];
	std::string name=j["name"];
	std::cout<<d <<","<<bl<<","<<name<<std::endl;
	return 0;
}

```

