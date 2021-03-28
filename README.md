# 个人站


## 分支说明
- `master`：主分支，站点部署分支
- `source`：默认分支，源文件分支


## 新建文章

本地切换到 `source` 分支，更新远程分支到本地

在 /source/_posts/ 文件夹下手动新建 .md 文件，或者输入以下命令自动生成（路径：/source/_posts/）

```shell
hexo new "hello"
```

完成 .md 内容

## 本地运行预览
```shell
hexo s -g
```

访问 http://localhost:4000 预览


## 发布

本地预览没有问题后，就可发布到 Hexo 服务器

此时，会更新 `master` 分支

```shell
hexo d -g
```

访问 https://liujinge.github.io


## 保存源文件

发布没有问题后，推送 `source` 分支到远程，保存源文件
