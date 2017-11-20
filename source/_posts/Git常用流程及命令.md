---
title: Git 常用流程及命令
tags: Git
categories: Git
---


## 常用流程

```
git clone <版本库的网址>	克隆

git pull origin master	获取远程数据，在本地会默认一个master分支，不要在该分支上操作

git branch test		创建本地新分支test
git checkout test	切换到test分支
git add .		添加更新
git commit -m ""	提交代码到本地
git pull origin master	同步远程分支
git push origin test	提交本地分支数据到自己远程分支，第一次创建的时候，会自动在远程创建同名分支

git checkout master	切换到本地master
git pull origin master	获取远程master最新数据
git merge test		将test分支合并到当前分支（master）
git push origin master	提交到远程主分支
git checkout test	切换回本地自己分支
```

<!-- more -->

## 强制覆盖本地文件
```
git fetch --all  
git reset --hard origin/master	（master 可以更换为其他分支）
git pull origin master
```

## 回退到某个版本
```
git reset --hard 版本号
```

## Git远程仓库管理
```
git remote add origin git@ github.com:yourname/yoursite.git  设置远程仓库地址
git push -u origin master  客户端首次提交
```

## 取消多次输入账号密码
打开 .git 目录下的 config 文件，并修改url（注意 .git 是隐藏文件）
```
url = git@github.com:yourname/yourname.github.io.git （yourname 替换成自己的）
```
