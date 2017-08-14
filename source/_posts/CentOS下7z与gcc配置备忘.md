---
title: CentOS下7z与gcc配置备忘
date: 2017-08-14 15:15:57
tags:
---

> 未科学上网yum安装 报错
> 	No package p7zip available.

> 编译源码安装
> `make`
> 报错
> 	make[1]: g++: Command not found
>	make[1]: *** [myGetTickCount.o] Error 127
>	make[1]: Leaving directory `/usr/app/p7zip_9.20.1/CPP/7zip/Bundles/Alone'
>	make: *** [7za] Error 2

安装gcc与g++

`yum install gcc-c++`
