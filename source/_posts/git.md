---
layout: post
title: "git学习笔记"
tags: [git]
date: 2018-7-13
comments: true
---

### 版本控制系统

版本控制系统分为集中式和分布式。

#### 集中式版本控制系统

工具：SVN、CVS

工作模式：先从中央服务器获得最新版本，然后开始写代码，最后把自己的代码再推送给中央服务器。

缺点：必须联网才可工作（可以是局域网）。

#### 分布式版本控制系统

工具：git

工作模式：没有中央服务器，每个人的电脑上都是一个完整的版本库，共同开发的时候，只需将各自的修改推送给彼此就好了。为了更加方便开发者交换修改内容，分布式也会有一台电脑充当中央服务器。

优点：比集中式更加安全、不比联网也可以工作、强大的分支管理

### 安装git--windows

首先去[git官网下载](https://git-scm.com/downloads)按照默认选项安装即可。

安装完成之后，在Git Bash中'自报家门'：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
注： 使用--global参数表示电脑上的所有仓库都用这个配置。

### git操作的基本命令

1. 初始化git： 在所需的文件夹中使用`git init`命令可以将该文件夹变成git管理仓库。

2. 当文件夹新增文件之后 使用`git add `命令来将文件添加至版本库，如修改了demo1.html：
```
$ git add demo1.html
```
3. 添加至版本库之后，使用`git commit`命令将文件提交到仓库。

```
$ git commit -m "wrote a demo1 file"
```
注： -m参数后面是本次提交时的备注。

### git操作的删除命令
1. `rm demo.html`：删除文件
2. `git rm demo.html` `git commit -m 'confirm delete'`：确认删除文件
3. `git checkout -- demo.html`：将删除的文件恢复到最新版本

### git操作常用命令

1. `git  status`：查看仓库当前的状态

2. `git diff demo.html` ：查看某文件具体修改了什么内容

3. `git log`：查看从最近到最远的提交日志

    `git log --pretty=oneline`：查看从最近到最远提交时的备注，可以看到版本号

4. `git reset --hard HEAD^`：使当前版本回归到上一个版本
    
   `git reset --hard commit_id`：hard后面加特定版本号（前几位就好），则回到特定版本

5. `git reflog`：查看每次操作的命令

### 工作中遇见的问题及解决方案
修改或新增文件后，没有`git pull -f --all`

然后`git add .  | git commit -am "init"`

导致github上的版本里有文件和本地版本冲突，下面给出冲突原因：

```
error: failed to push some refs to 'https://gitee.com/junyaokeji/quanzidaihuan.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```
解决办法是`git push -u origin master -f`强制覆盖已有的分支（可能会丢失改动）
