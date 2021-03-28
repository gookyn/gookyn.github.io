---
title: gitignore文件不起作用
tags: Git
categories: Git
---

## 问题


.gitignore中已经标明忽略的文件目录下的文件，git push的时候还会出现在push的目录中，或者用git status查看状态，想要忽略的文件还是显示被追踪状态。


## 原因


在git忽略目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的。


## 方案


先把本地缓存删除，然后再进行git的提交，这样就不会出现忽略的文件了。


## 解决


> git清除本地缓存（改变成untrack状态），然后再提交



```shell
# 清空所有文件状态（注意有 . ）
$ git rm -r --cached .

# 或者清空指定目录或文件状态（xxx为目录或者文件路径）-- 这个更实用
$ git rm -r --cached xxx

# 暂存
$ git add .

$ git commit -m 'update .gitignore'
```


## 注意


- .gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
- 想要.gitignore起作用，必须要使这些文件不在暂存区中才可以，.gitignore文件只是忽略没有被staged(cached)文件。
- 对于已经被staged文件，加入.gitignore文件时一定要先从staged移除，才可以忽略。
