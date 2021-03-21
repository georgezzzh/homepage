---
title: C/C++中正则表达式的使用
date: 2019-10-29 22:04:20
tags:
- C/C++
- 正则表达式
---

#### C语言实现(标准库)

环境Linux, 无需其他依赖, 注释应该蛮详细了.
<!--more-->

```c
#include <stdio.h>
#include <regex.h>
int main() {
    const char* pattern="(CN)([0-9]+)(.*)";
    char *str="CN86US01";
    regex_t reg;
    //编译正则表达式
    regcomp(&reg,pattern,REG_EXTENDED);
    regmatch_t pmatch[4];
    //nmatch为要匹配的数目,是要匹配的group,例如上式有3个group(CN),group([0-9]+),(.*),然后算group(0)一共是4
    //pmatch传一个regmatch_t数组, eflags一般为0(具体查看man regexec中有写详细的参数)
    int status=regexec(&reg,str,4,pmatch,0);
    //表示匹配成功
    if(status==0){
        //浏览所有匹配到的group
        for(size_t i=0;i<reg.re_nsub+1;i++) {
            //rm_so是字符串匹配的起始位置,rm_eo是字符串匹配的终止位置
            for (int j = pmatch[i].rm_so; j < pmatch[i].rm_eo; j++){
                printf("%c", str[j]);
            }
            printf("\n");
        }
    }
    //释放正则表达式
    regfree(&reg);
    return 0;
}
```

#### C++中实现(标准库)

```cpp
#include <iostream>
#include <regex>
int main() {
    std::regex pattern("([\\w]+)@(\\w+).com");
    std::string str="whoever@qq.com";
    std::smatch result;
    bool is_match=std::regex_match(str,result,pattern);
    if(is_match){
        std::cout<<"匹配成功:\t"<<result[0]<<std::endl;
        //将每个group打印出来
        for(size_t i=1;i<result.size();i++){
            std::cout<<result[i]<<std::endl;
        }
    }else{
        std::cout<<"匹配失败"<<std::endl;
    }
    return 0;
}
```


正则表达式是什么就不介绍了.
**我实在是懒得吐槽了,中文博客搜遍也找不到如何找出正则表达式所匹配到的group, 把一份博客翻来覆去地抄是真的没节操.**




