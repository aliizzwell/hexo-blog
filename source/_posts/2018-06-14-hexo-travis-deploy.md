---
title: 使用travis自动部署hexo博客到github pages
date: 2018-06-14 13:08:14
tags:
---

之前的博客一直用的是wordpress，因为hexo的话，每次还得手动运行`hexo generate`，然后把public文件夹复制到腾讯云上。如果是使用github pages的话，也需要每次把public文件夹push上去。就感觉很繁琐，觉得还不如用wordpress，在线写文章。

后来，越来越觉得在线些文章是个很蛋疼的事情。而且，wordpress使用了数据库来存储文章，就感觉很不优雅。于是开始考虑换回hexo。

<!--more-->

关于hexo部署繁琐的问题，最近接触了持续集成(CI, Continuous integration)，感觉可以用于自动部署，于是研究了一下，成功了。

目前使用的持续集成工具是[travis](https://travis-ci.org/)，使用github pages托管静态页面。现在的效果是，只要往github上一推送，就可以自动generate并将public文件夹push到gh-pages分支，实现github pages的自动更新。

# 原理

持续集成的原理是，每次检测到github上的提交，就运行一些预设的命令。
这些命令定义在仓库根目录下的`.travis.yml`文件中。
因此，只要合理配置这些命令，就可以让travis帮你完成generate和deploy的工作。

另一个需要解决的问题是，要让travis推送到你的github仓库，需要它有读写权限。
但是，我们总不能把github的密码写入配置文件中去吧？更不能把ssh密钥写入配置文件中。
好在，travis提供了加密功能，可以加密一个字符串，然后把加密的字符串写入配置文件中。这样就只有travis和你自己知道原始字符串的内容了。

另外，要让travis拥有读写仓库的权限，其实并不需要给它账号密码，或者ssh密钥。只需要给他一个personal access token即可。而且，这样还可以精确地控制travis的权限，而不是给它所有的用户权限。

# 方法
