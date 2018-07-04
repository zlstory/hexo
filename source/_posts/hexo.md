---
layout: post
title: "hexo个人博客"
date: 2017-8-24
tags: [hexo]
comments: true
---

上一篇博客更新的时间是7月25，今天已经是8月24了，一个月都没更新博客了，不能说明这一个月没有学到新的知识点，只能说这个月的事情太多了，这是值得纪念的一个月。本来想总结一下，但是又怕觉得矫情，不过昨天刚结束完公司的事情，自己的事情还一大堆。由于换了新电脑，hexo的环境要重新再建，折腾了一上午，现在趁着还记得操作步骤，把他记下来，不然下次换电脑，又要开始重新找资料。（一个月不操作，md文档都不会了，还是看着文档写的。）

###  在E盘下输入命令
```javascript
    npm install -g hexo-cli
    hexo init fileName  
    cd fileName
    npm install
```
### 换主题
我用的主题是[maupassant](https://www.haomwei.com/technology/maupassant-hexo.html)，首先在fileName中输入以下命令
```javascript
    $ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
    $ npm install hexo-renderer-jade@0.3.0 --save
    $ npm install hexo-renderer-sass --save
```
### 配置fileName文件下的_config.yml主要参数，其他地方默认即可
```javascript
    title: zlstory
    subtitle: 
    description: 个人学习小结
    author: zilan
    language: zh-CN
    timezone: Asia/Shanghai

    url: http://zlstory.com
    root: /
    permalink: :year/:month/:day/:title/
    permalink_defaults:

    theme: maupassant

    deploy:
    type: git
    repository: git@github.com:zlstory/zlstory.github.io.git
    branch: master

    search:
    path: search.xml
    field: post

```
### 配置maupassant目录下的_config.yml文件
```javascript
    fancybox - 是否启用Fancybox图片灯箱效果
    disqus - Disqus评论 shortname
    gitment - Gitment评论相关参数
    uyan - 友言评论 id
    livere - 来必力评论 data-uid
    changyan - 畅言评论 appid
    google_search - 默认使用Google搜索引擎
    baidu_search - 若想使用百度搜索，将其设定为true。
    swiftype - Swiftype 站内搜索key
    tinysou - 微搜索 key
    self_search - 基于jQuery的本地搜索引擎，需要安装hexo-generator-search插件使用。
    google_analytics - Google Analytics 跟踪ID
    baidu_analytics - 百度统计 跟踪ID
    show_category_count - 是否显示侧边栏分类数目
    toc_number - 是否显示文章中目录列表自动编号
    shareto - 是否使用分享按鈕，需要安装hexo-helper-qrcode插件使用
    busuanzi - 是否使用不蒜子页面访问计数
    widgets_on_small_screens - 是否在移动设备屏幕底部显示侧边栏
    canvas_nest - 是否使用canvas动态背景
    donate - 是否启用捐赠按钮
    menu - 自定义页面及菜单，依照已有格式填写。填写后请在source目录下建立相应名称的文件夹，并包含index.md文件，以正确显示页面。导航菜单中集成了FontAwesome图标字体，可以在这里选择新的图标，并按照相关说明使用。
    widgets - 选择和排列希望使用的侧边栏小工具。
    links - 友情链接，请依照格式填写。
    timeline - 网站历史时间线，在页面front-matter中设置layout: timeline可显示。
    Static files - 静态文件存储路径，方便设置CDN缓存。
    Theme version - 主题版本，便于静态文件更新后刷新CDN缓存。

```
我用的是本地搜索，即self_search:true,需要在fileName中安装一个jq的插件， npm install hexo-generator-search --save，另外disqus需翻墙使用。

### 将hexo与github相连接
就是为了hexo d的时候，能直接更新github中的内容，首先[配置ssh](http://blog.csdn.net/binyao02123202/article/details/20130891)
```javascript
    $ ssh-keygen -t rsa -C "your_email@example.com"
```
然后一直按enter，然后生成三个文件，默认目录是C:\Users\Administrator\.ssh，将id_rsa.pub放在github的SSH和GPG密钥中。再配置git的用户名和密码
在使用hexo -d之前 要安装一个插件：npm install hexo-deployer-git --save

### 域名的绑定
新建一个CNAME文件，内容就是www.zlstory.com,放在source文件夹下，不能放在fileName文件夹中，否则hexo d的时候会消失，神奇。还有要安装rss插件，虽然我不知道有什么用这个东西，虽然有人解释过，但是他出现了，那就装一下吧：npm install hexo-generator-feed --save，然后再theme的配置文件中加入rss: /atom.xml。

备注：这篇文章不是为了教你怎么利用hexo加github搭建博客的，只是为了我自己下次换电脑方便，哈哈哈~

