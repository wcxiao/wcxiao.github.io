---
layout: post
title: 'Jekyll本地预览网页not found'
date: 2020-04-27
author: wcxiao
color: rgb(255,210,32)
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: jekyll
---

## 问题描述

执行bundle exec jekyll serve，打开127.....后查看博客出现404 not found；

文件确实存在于_site文件夹，但控制台显示：

![error](https://picholder.oss-cn-shanghai.aliyuncs.com/jekyll/notfound.png)

## 解决方案

备份C:\Ruby25-x64\lib\ruby\2.5.0\webrick\httpservlet\filehandler.rb，然后将其打开，修改两个地方：

第285行path = req.path_info.dup.force_encoding(Encoding.find("filesystem"))之后加上：
path.force_encoding("UTF-8")

第333行break if base == "/"之后加上base.force_encoding("UTF-8")

保存后重新执行jekyll，解决问题；

reference:

https://blog.csdn.net/u010632165/article/details/103538062