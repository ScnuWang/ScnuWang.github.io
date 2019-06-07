看到好几个很厉害的博主都是使用的这个主题，自己也是对这款主题青睐已久，但是之前尝试了好几次都没有完成的使用成功过，尝试了直接去照搬其他博主的仓库下来修改，不是这个依赖过时，就是那个依赖过时，对Ruby也不在行，所以经历千辛万苦还是没有成功，这次再鼓起勇气尝试，终于成功了。

本地使用的Ruby的版本是：Ruby 2.5、gem 2.7.6.2

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