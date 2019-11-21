---
layout: post
title: "hexo-BlueLake主题"
date: 2018-7-12
tags: [hexo]
comments: true
---

好久没写博客了，突然收到阿里云的信息，问我是否需要续费域名，觉得还是继续吧。

虽然很喜欢之前的maupassant主题，但是发现这个主题出现了某种问题，所以换成了[BlueLake](https://github.com/chaooo/hexo-theme-BlueLake);


### 安装主题
```javascript
$ git clone https://github.com/chaooo/hexo-theme-BlueLake.git themes/BlueLake
$ npm install hexo-renderer-jade@0.3.0 --save
$ npm install hexo-renderer-stylus --save
```
### 配置fileName文件下的_config.yml主要参数，其他地方默认即可
```yml
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

    theme: BlueLake

    deploy:
    type: git
    repository: git@github.com:zlstory/zlstory.github.io.git
    branch: master

    search:
    path: search.xml
    field: post

```
### 配置BlueLake目录下的_config.yml文件
```yml
version: 2.0.1
menu:
  - page: home
    directory: .
    icon: fa-home
  - page: archive
    directory: archives/
    icon: fa-archive
  - page: about
    directory: about/
    icon: fa-user
  - page: rss
    directory: atom.xml
    icon: fa-rss
widgets:
  - recent_posts
  - category
  - tag
  - archive
  #- weibo
  - links

toc:
  enable: true
  number: false

js: js
css: css

date_formats:
  archive: "MM月DD日"
  category: "YYYY/MM/DD"
  post: "MMM DD, YYYY"
  tag: "YYYY/MM/DD"

Plugins:
  hexo-generator-feed
  hexo-generator-sitemap
  hexo-generator-baidu-sitemap

feed:
  type: atom
  path: atom.xml
  limit: 20

sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml

#Local search
local_search: true ## Use a javascript-based local search engine, true/false.

#Cmments
comment:
  duoshuo: #chaooo ## duoshuo_shortname
  disqus: ## disqus_shortname
  livere: ## 来必力(data-uid)
  uyan: ## 友言(uid)
  cloudTie: ## 网易云跟帖(productKey)
  changyan: ## 畅言需在下方配置两个参数，此处不填。
    appid: ## 畅言(appid)
    appkey: ##畅言(appkey)
  gitment:
    enable: true ## If you want to use Gitment comment system please set the value to true.
    owner: zlstory ## Your GitHub ID, e.g. username
    repo: zlstory.github.io ## The repository to store your comments, make sure you're the repo's owner, e.g. imsun.github.io
    client_id: 2beb49205958bc8a66be ## GitHub client ID, e.g. 75752dafe7907a897619
    client_secret: 1e8e5079085aa3df305c2e00d0ba45a326992962 ## GitHub client secret, e.g. ec2fb9054972c891289640354993b662f4cccc50

#Share
baidu_share: false ## 百度分享
JiaThis_share: ##true ##JiaThis分享
duoshuo_share: #true ##true 多说分享必须和多说评论一起使用。
addToAny_share: # AddToAny share. Empty list hides. List items are service name at url. For ex: email for '<a href="https://www.addtoany.com/add_to/email?linkurl=...'
#  - twitter
#  - baidu
#  - facebook
#  - google_plus
#  - linkedin
#  - email

# Analytics
google_analytics:  UA-111606593-1 ## Your Google Analytics tracking id, e.g. UA-42025684-2
baidu_analytics: 68cb98bd2a475f71ad8508d14ad906ba ## Your Baidu Analytics tracking id, e.g. 1006843030519956000

# Miscellaneous
show_category_count: true ## If you want to show the count of categories in the sidebar widget please set the value to true.
widgets_on_small_screens: true ## Set to true to enable widgets on small screens.
busuanzi: true ## If you want to use Busuanzi page views please set the value to true.

# About page
about:
  photo_url: /img/avatar.png ## Your photo e.g. http://obzf7z93c.bkt.clouddn.com/themeauthor.jpg
  items:
  - label: email
    url: ## Your email with mailto: e.g.  mailto:zhenggchaoo@gmail.com
    title: ## Your email e.g.  zhenggchaoo@gmail.com
  - label: github
    url:  https://github.com/zlstory ## Your github'url e.g.  https://github.com/chaooo
    title: zlstory ## Your github'name e.g.  chaooo


```
我用的是本地搜索，即self_search:true,需要在fileName中安装一个jq的插件， npm install hexo-generator-search --save

### 将hexo与github相连接
就是为了hexo d的时候，能直接更新github中的内容，首先[配置ssh](http://blog.csdn.net/binyao02123202/article/details/20130891)
```javascript
    $ ssh-keygen -t rsa -C "your_email@example.com"
```
然后一直按enter，然后生成三个文件，默认目录是C:\Users\Administrator\.ssh，将id_rsa.pub放在github的SSH和GPG密钥中。再配置git的用户名和密码
在使用hexo -d之前 要安装一个插件：npm install hexo-deployer-git --save

ssh是可以绑定多个的。

### 域名的绑定
新建一个CNAME文件，内容就是www.zlstory.com,放在source文件夹下，不能放在fileName文件夹中，否则hexo d的时候会消失，神奇。还有要安装rss插件，虽然我不知道有什么用这个东西，虽然有人解释过，但是他出现了，那就装一下吧：npm install hexo-generator-feed --save，然后再theme的配置文件中加入rss: /atom.xml。

备注：此网址更详细的介绍了 BlueLake主题的配置：http://chaoo.oschina.io/2016/12/29/BlueLake%E5%8D%9A%E5%AE%A2%E4%B8%BB%E9%A2%98%E7%9A%84%E8%AF%A6%E7%BB%86%E9%85%8D%E7%BD%AE.html

