---
title: vps备忘
date: 2017-08-17 13:42:24
tags:
---

##　关于URL redirect和URL frame的区别
这是两种不同的URL转发模式，第一种：URL redirect是直接转发，也就是你访问域名后转向后显示的是转向后的域名，也就是你从AURL转到B之后，显示的是B的域名。
第二种URL frame 就是隐藏转发，转发后显示的域名还是A域名！客户可以根据自己的需要使用相应的功能！

## ssh
`ssh root@11.11.11.11 -p xxxx` -p 端口号

## 防火墙与端口

ssr-go 版脚本开启了防火墙，手动打开ssh xxxx口、ng 80口、hexo 4000口，并**重启**防火墙生效。
