---
title: Linux常用命令-文件与目录操作
tags: Linux
categories: Linux
---

## `pwd`


> print working directory：查看当前目录



```shell
pwd
```


## `cd`


> change directory：切换目录



```shell
# 切换目录
cd home

# 切换到上一级目录
cd ..

# 切换到根目录
cd /

# 切换到当前用户目录
cd
# or（~代表当前用户主目录）
cd ~
```


## `ls`


> list：查看当前目录中的文件列表



```shell
# 查看当前目录中的文件夹和文件
ls

# 查看某个目录中的文件夹和文件，如tmp
ls tmp
```


## `mkdir`


> make directory：创建目录或文件



```shell
# 创建单个目录
mkdir dir0

# 查看某个目录中的文件夹和文件，如tmp
ls tmp

# 在某个目录下创建
mkdir /tmp/tutorial

# 创建多个目录
mkdir dir1 dir2 dir3

# 创建逐层目录
mkdir -p dir4/dir5/dir6
```


## `echo`


> echo：打印



```shell
# 打印到控制台
echo "This is test"

# 打印到某个文件
echo "test1" > test_1.txt
```


## `cat`


> concatenate：链接；查看文件



```shell
# 查看单个文件
cat test_1.txt

# 查看多个文件
cat test_1.txt test_2.txt test_3.txt
cat test_?.txt
cat test_*

# 多个文件内容整合到一个文件
cat t* > combined.txt
cat combined.txt
```


注意：
重复执行命令会覆盖已经存在的文件，如果需要追加内容而不是替换内容，可用多个">>"：


```shell
# 追加整个内容
cat t* >> combined.txt

# 再追加一行
echo "Add a line" >> combined.txt

cat combined.txt
```


## `less`


> less：文件内容过多时，以分页方式查看



```shell
# 查看单个文件
less combined.txt
```


## `q`


> quit：退出less查看模式



```shell
q
```


## `mv`


> move：移动文件或目录；修改文件名或目录名



```shell
# 移动文件到某个目录
mv combined.txt dir1

# . 代表当前目录
mv dir1/combined.txt .

# 移动多个文件到一个目录，最后一个为目标目录，如把以下几个文件及文件夹dir3移动到dir2
mv combined.txt test_* dir3 dir2

# 修改文件名、目录名
mv test_1.txt test1.txt
mv "folder 1" folder_1
```


## `cp`


> copy：复制文件或目录



```shell
# 复制文件
cp combined.txt combined_backup.txt

# 复制文件到其他目录
cp combined.txt dir1
```


## `rm`


> remove：移除文件或目录



```shell
# 删除单个文件
rm combined_backup.txt

# 删除某个目录中的文件
rm dir4/dir5/dir6/combined.txt

# 删除多个文件
rm combined_backup.txt dir4/dir5/dir6/combined.txt
rm test_*.txt

# 强制删除某个目录及目录中所有文件
rm -r dir4

# 带提示删除（Y or N）
rm -i combined.txt
```


## `rmdir`


> remove directory：删除目录



```shell
# 删除目录（注意只会删除空目录）
rmdir folder_1
rmdir dir*
```


## `|`


> pipe 管道，将一个命令传递到另一个命令



## `wc`


> count：统计数量



- `-l`：line 行数或文件数



```shell
# 查看文件行数
wc -l combined.txt

# 查看home目录文件数
ls ~ | wc -l

# 以分页形式打开etc目录
ls /etc | less
```


## 其他


- `man`：manual 查看操作手册
- `uniq`：unique 在文件中输出不重复的行
- `sort`：排序
- `reset`：清空当前窗口内容
- `whoami`：查看当前用户名
- `sudo`：超管身份运行
