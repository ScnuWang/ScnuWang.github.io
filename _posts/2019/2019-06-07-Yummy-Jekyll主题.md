---
layout: post
title: Yummy-Jekyll主题
category: jekyll
date: 2019-06-07 12:00
tags: [主题]
---

终于安装成功了！！！

看到好几个很厉害的博主都是使用的这个主题，自己也是对这款主题青睐已久，但是之前尝试了好几次都没有完成的使用成功过，尝试了直接去照搬其他博主的仓库下来修改，不是这个依赖过时，就是那个依赖过时，对Ruby也不在行，所以经历千辛万苦还是没有成功，这次再鼓起勇气尝试，终于成功了。

本地使用的Ruby的版本是：Ruby 2.5、gem 2.7.6.2

### 安装主题

0、下载主题：<https://github.com/DONGChuan/Yummy-Jekyll>

1、安装[Bower](http://bower.io/) 

```
npm install -g bower
```

2、运行命令

```
bower install
bundle update # 更新依赖
bundle install
```

3、修改`_config.yml`

4、发布文件放到`/_posts`目录下

### 添加Disqus

注意：这个工具需要有VPN的网络环境下才能正常使用。

1. 注册一个Disqus账号

2. 在Disqus上添加网站，会有一个已disqus.com结尾的账号，添加到`_config.yml`中disqus对应的配置处即可

3. 在Disqus网站上添加白名单

   ![1559897796774](https://scnuWang.github.io/assets/images/1559897793753.png)

### 使用技巧

1. 文件名添加时间，可以自动识别为文章的时间，也可以在头部添加`date: 2019-06-07`属性,如果两个都存在，以date属性的为准。
2. tags属性可以添加多个。
3. 文章中的图片同意保存在`/assets/images`下面，地址前需要加上博客的地址`xxx.github.io`。
4. 如果不想要右边的侧边栏，可通过在头部添加属性`no-post-nav: true`即可。