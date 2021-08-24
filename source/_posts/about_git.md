---
layout: 
title: git相关知识
date: 2021-08-24
tags:
- git
- github
categories:
- [学习, coding]
---

记录学习git的一些问题
<!-- more -->

###  1 工作区、暂存区、版本库
基本概念：
- **工作区**：PC中的目录
- **暂存区**：称为stage或index，一般存放在`.git`目录下的index文件中，故也称其为索引
- **版本库**：工作区的隐藏文件`.git`不算工作区而是版本库
&emsp;&emsp;add指令将工作区提交到暂存区，commit指令将暂存区提交到版本库。

### 2 Git基本操作
预先说明，几个库：*workspace*, *staging area*, *local repository*, *remote reposity*
- **git pull**：*remote reposity* ->*workspace* 
- **git add**：*workspace*->*staging area*
- **git commit**：*staging area*->*local repository*
- **git push**：*local repository*->*remote reposity*
- git fetch/clone：*remote reposity*->*local repository*
- git checkout：*local repository*->*workspace*
- git reset：回退版本
- git rm：删除工作区文件

远程操作
- **git remote**
添加远程库：`git remote add [shortname] [url]`这里的shortname一般为origin，指代的就是url的远程库，一般把github作为远程库。
更改仓库名：`git remote rename old_name new_name`
- **git pull**
从远程获取并合并在本地： `git pull <远程主机名> <远程分支名>:<本地分支名>`当与本地的当前分支合并时可以省略本地分支名。
其等价于 `git fech`和 `git merge`
- **git push**
上传本地分支版本至远程并合并：`git push <远程主机名> <本地分支名>:<远程分支名>`
如果本地分支名与远程分支名相同，则可以省略冒号 `git push <远程主机名> <本地分支名>`

### 3 git 分支管理
此部分为git最有特点的一部分...