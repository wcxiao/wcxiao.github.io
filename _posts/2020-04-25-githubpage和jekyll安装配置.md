---
layout: post
title: 'Github Page and Jekyll搭建'
date: 2020-04-25
author: wcxiao
color: rgb(255,210,32)
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: jekyll
---

平台: windows10  
时间：2020-04-25

## 创建GitHub page

创建仓库，如图：
![p2](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/newrepo.png)

repository name最好是username.github.io(username是你的github用户名)，这样可直接使用该网址username.github.io访问博客。如果不是github的付费用户则最好选择public，只有付费用户的private产生的github page才能被别人访问。如果该仓库只存放博客，则只要默认分支即可，如果还放其它东西，可以建立另一个分支。

在本地新建一个文件夹存放博客，把创建的repo克隆到该文件夹下：
![p3](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/buildpage1.png)

然后在本地仓库文件夹创建一个index.html文件：
index.html:
```html
<!DOCTYPE html>
<html>
	<head>
	My Blogs
	</head>
	<body>
	<p>hello, my first page!</p>
	</body>
</html>
```

然后push到仓库：
![p4](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/buildpage2.png)

之后访问username.githu.io即可查看
![p5](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/blogpage.png)

到这一步只是显示了一个基本页面，下面安装jekyll进行高级操作

可以先下载jekyll主题([链接](http://jekyllthemes.org/))

将下载好的主题解压到本地仓库，覆盖原文件，保留.git文件夹。如图：
![themefiles](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/themefiles.png)

可以看到文件中有Gemfile和Gemfile.lock两个文件，后面会提到

## 软件安装

### 安装Git

[下载地址](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git/)

安装步骤可参考该[博客](https://blog.csdn.net/sanxd/article/details/82624127)

### 安装Jekyll

需要先安装Ruby和gem

[下载地址](https://rubyinstaller.org/downloads/)(可直接下载Ruby-Devkit的包)

安装步骤可参考:

https://www.jianshu.com/p/58e2c5ea3103  
https://blog.csdn.net/mouday/article/details/79300135

![p1](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/ruby_msys2.png)

安装好ruby和devkit后(需要安装msys2,及上图所示的1，3)，进入cmd(可以用Git cmd)，使用gem -v查看是否安装成功

然后换源：(参考[网站](https://gems.ruby-china.com/))

gem sources -l

gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

安装Bundle：(bundle版本应该与gemfile.lock中的版本要求一致)

gem install bundle

bundle config mirror.https://rubygems.org https://gems.ruby-china.com

安装jekyll:

gem install jekyll

jekyll --version

安装好后进入仓库文件夹，即gemgile所在文件夹，运行

Bundle install

该步骤会自动根据gemfile中的配置安装好依赖

这一步完成就安装好了

## 启动服务

然后在这里打开git cmd启动jekyll服务：

bundle exec jekyll serve

访问http://127.0.0.1:4000进行预览

再通过git bash将更改推送到github

之后访问username.github.io查看效果

之后更新博客只需直接将markdown文件放到_posts文件夹下，然后设置好标题，再在本地启动jekyll服务进行编译，最后使用git进行提交就行了

还可以绑定自己的域名
具体步骤可以参考:

在github仓库设置中添加custom domain，然后在阿里云控制台进行域名解析：
![p6](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/domain.png)

## reference:

安装jekyll和配置GithubPage：https://www.cnblogs.com/sqchen/p/10757927.html

域名绑定可参考：https://blog.csdn.net/FlowerDance17/article/details/80685112

RubyGem镜像：https://gems.ruby-china.com/

windows安装jekyll常见问题：

https://blog.csdn.net/mouday/article/details/79300135

https://www.jianshu.com/p/58e2c5ea3103

jekyll主题下载：http://jekyllthemes.org/

推荐：

如何让百度收录 GitHub Pages 个人博客：https://zhuanlan.zhihu.com/p/111773896