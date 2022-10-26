---
title: gitee
date: 2020-09-08 22:23:28
tags: 
	- git
	- gitee
---

# 码云的使用

### 提交没有显示贡献问题

> 问题1:提交后贡献度不显示
>
> 原因1:本地git和码云帐号邮箱不一致
>
> 解决1:将本地帐号调整一下
>
> 查看当前git用户名： git config user.name
>
> 查看当前git邮箱： git config user.email
>
> 切换git用户名: git config --global user.name "YOURUSERNAME"
>
> 切换git邮箱： git config --global user.email "YOUREMAIL"

## clone代码

`git clone git@gitee.com:guanruihua/grh_note.git`

## 上传文件

```git
git add .(上传全部文件)
git commit -m "备注"(上传到缓冲区)
git push origin master(上传到云端)
```

## 删除文件

```
rm '文件名'(删除文件)
git add '文件名'(将删除文件的状态添加到暂存区)
git commit -m 'delete 文件名'(将改文将状态上传到缓冲区)
git push (上传到云端)
```

## 删除文件夹

```git
ls (查看本地分支的文件)
git rm raindow -r -f (删除raindow文件夹及其所有文件)
git commit -m "delete raindow" (同步删除操作)
git push origin master (提交分支)
```

密钥对创建成功。

指纹：F0A136C885F5A7438B624048CB2352B6BB958F9A

## 删除分支

```git
git branch -a (查看所有分支)
git push origin --delete HEAD (删除HEAD分支)
```
