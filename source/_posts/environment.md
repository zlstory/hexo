---
layout: post
title: "重装系统之后的一系列操作"
date: 2018-11-19
tags: [environment]
comments: true
---

##### 安装[node环境](https://nodejs.org/zh-cn/) 


##### 安装[git环境](https://git-scm.com/downloads)
 下载安装之后，执行以下命令
 ```
$ git config --global user.name "zlstory"

$ git config --global user.email "13968106594@163.com"

$ ssh-keygen -t rsa -C "13968106594@163.com"

 ```
 创建ssh密钥之后，将github中setting中的SSH and GPG keys新增。title随便填，下面的把刚才生成的id_rsa.pub（目录：C:\Users\Crystal\.ssh）用记事本打开把内容贴进去。

 ##### 安装hexo环境

 ```
npm install -g hexo-cli

$ ssh-keygen -t rsa -C "13968106594@163.com"

 ```
 将之前保存的hexo压缩包解压之后，直接`npm install`即可本地预览。


##### host文件地址

```
C:\Windows\System32\drivers\etc

```