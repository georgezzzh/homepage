---
title: git仓库瘦身
date: 2019-11-23 21:52:16
author: george
tags:
-  Git
---
tips: release中的内容也会被clone下来
### 1. 清除历史中跟踪某个大文件
1. 找出其中最大的三个文件
```
git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -3
```
2. 查看大文件
```
git rev-list --objects --all | grep 第一步的哈希码
```
3. 移除对该文件的引用
```
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch 'JAVA_API_1.8.CHM'"  --prune-empty --tag-name-filter cat -- --all
```
4. 进行repack
```
git for-each-ref --format='delete %(refname)' refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now

```
5. 查看pack空间使用情况
```
git count-objetcs -v -H
```
6. 强行提交
```
git push origin master --force

```

### 重置仓库

```
rm -rf .git
git init
git add -A
git commit -m "清除所有历史"
git add remote xxx.git 
git push origin master --force
```
### 只克隆近几次的提交
```
# 只克隆3层提交
git clone xxx.git --depth 3
```

>参考： https://www.zhihu.com/question/29769130 
>
