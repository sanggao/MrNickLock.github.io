---
title: Travis-CI自动部署Hexo记录
date: 2017-06-12 14:01:01
tags:
	- 记录
	- 配置
---

记录实验过程中踩的坑和收获。

过程参考[文](http://blog.csdn.net/woblog/article/details/51319364)

> 本地配置并测试过了Hexo + Material，为了方便日后的跨设备更博，遂尝试Travis-CI自动部署

远程github库中新建blog-source分支，用以存放源文件。
本地已有的Hexo文件夹中，`git init`然后一通关联远程，然后push
失败，失败，失败……

## push 失败

暂时移走blog文件夹下的全部内容，克隆远程库，生成如下目录结构：

- blog
	+ MrNickLock.github.io
		* .git
		* 其他内容

直接把全部内容移到blog下

- blog
	+ .git
	+ 其他内容
	+ 删除空目录MrNickLock.github.io

**Get!**
应该主要是 .git 目录的锅，再push，done！

## material主题集成失败

这是material主题配置文件被忽略，push到github的blog-source文件中根本没有_config.yml，修改material目录下 .gitignore 文件，解除忽略（todo
：再进一步了解.gitignore文件的作用）

> 关于刷新忽略文件，见上篇博文。