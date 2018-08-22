---
title: CentOS Kettle 部署记录
date: 2017-08-21 17:04:36
tags: Kettle
---

Kettle 作为开源 ETL 工具中的翘楚（之一），依赖 Java 实现跨平台。 Windows 下的部署很傻瓜式， Linux 的部署过程如下文。

PS: Linux 下启动 Kettle 仓库管理常见**白屏异常**，解决方式见文末。

- 查看 Linux 自带的 JDK 版本（默认为 OpenJDK , 建议安装完整的 OracleJDK ）

```bash
rpm -qa|grep java
```

- 移除现有 Java 包，并安装 完整的 OracleJDK

```bash
yum remove java*
```



```bash
tar -zxvf jdk8.tar.gz
```


```bash
vim /etc/profile
```


```bash
source /etc/profile
```

- 尝试启动

```bash
spoon.sh
```

在连接管理仓库时**白屏异常**

> 
> WARNING:  no libwebkitgtk-1.0 detected, some features will be unavailable
>    Consider installing the package with apt-get or yum.
>    e.g. 'sudo apt-get install libwebkitgtk-1.0-0'


缺少 `webkitgtk` （无法完成页面渲染，so，白屏），警告提示我们 `sudo apt-get install libwebkitgtk-1.0-0` 来安装。 But,这是 debian 、 Ubuntu 常用包安装方式。 CentOS 可以这样：

```bash
yum install epel-release
```

```bash
yum install webkitgtk
```

其中，`epel` 是个安装软件包的好东西，可以去了解一下。

> Windows 下 的页面渲染调用的是系统自带的 IE 。


[from](https://unix.stackexchange.com/questions/182603/how-to-install-webkitgtk1-on-rhel7)
