---
title: Hadoop1.2.1集群搭建
date: 2017-08-18 10:52:07
tags:
---

[toc]
# hadoop1.2.1集群搭建

> 环境：虚拟机：VM12（win10）操作系统：CentOS7（最小安装）

## 1.IP与主机名：
> 方便远程ssh工具登录

- 配置静态IP

查看现有网络配置

```
[root@nick ~]# ifconfig 
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.203  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::c472:73c0:1189:2edf  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ac:04:6d  txqueuelen 1000  (Ethernet)
        RX packets 178665  bytes 31856422 (30.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 75340  bytes 16564447 (15.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 97995  bytes 24766840 (23.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 97995  bytes 24766840 (23.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:64:2a:07  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

**ens33** 为默认网卡，更改其配置文件（vi /etc/sysconfig/network-scripts/ifcfg-ens33）如下：
```

[root@nick network-scripts]# cat ifcfg-ens33
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=" 局域网"
UUID=0d39ce5b-0cb0-434a-8dee-8783f9805112
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.1.203
PREFIX=24
GATEWAY=192.168.1.1
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```
> UUID 为唯一标识，不可重复，新建虚拟机会自动生成，克隆虚拟机此项重复，导致克隆机网络不通，解决方法
：删除UUID，在VM虚拟机网络配置高级选项中，重新生成mac地址,必须关闭虚拟机，再启动，新的mac才会生效，Done！

重启网络
```
systemctl restart network.service
```

- 关闭防火墙

> 防火墙会导致datanode 进程死亡！！！

```
systemctl stop firewalld.service --关闭防火墙

systemctl disable firewalld.service --永久关闭
```

- 修改主机名

>此目录 CentOS 7 下不同于CentOS 6

```
vi /etc/hostname

```
> PS：CentOS 6 下？

将默认的localhoat.domain全部修改为nick(自定义的新主机名)



- 绑定主机名与ip

```
vi /etc/hosts

192.168.1.202 long
192.168.1.202 nick
192.168.1.202 nick_1
192.168.1.202 nick_2
192.168.1.202 nick_3

```

> PS：CentOS 6 下？

其中，long为win主机，nick为主节点（masters），nick_1、nick_2、nick_3为三个次节点（slaves）

## 2.安装JDK

- 查看现有JDK
```
java -version
```
> CentOS 7 桌面版（或其他更高）自带openJDK，使用yum卸载方式，卸载全部

- 解压
- 配置环境变量

## 3.配置hadoop1.2.1

- 解压
- 配置环境变量
- 更改配置文件

    1. hadoop_1/conf/hadoop_env.sh

    
    ```

		#=============JAVA_HOME===================

         export JAVA_HOME=/usr/app/jdk

    	#=========================================
    
    
    
    	#=============warning=====================

        export HADOOP_HOME_WARN_SUPPRESS=1

    	#=========================================

    ```

    2. 



## 4.克隆虚拟机

## 5.ssh免密登录

> 哪有我的密钥，我就可以直接登录哪

```
ssh-keygen -t rsa

cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
```
 合并所有私钥authorized_keys
 
 即时生效，无需重启
 
 >  免密登录 报错
 
 ```
 ssh nick
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
32:f4:ee:a0:4c:01:29:d1:6c:29:7c:ca:2a:8c:8a:57.
Please contact your system administrator.
Add correct host key in /root/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /root/.ssh/known_hosts:5
ECDSA host key for nick has changed and you have requested strict checking.
Host key verification failed.
[root@nick_3 ~]# ssh nick_1
Last login: Fri Mar 17 14:28:42 2017 from nick

 ```
 
  对应删除 .ssh 目录下的 known_hosts 中的连接记录即可
  
  
## 6.hadoop 格式化

## 7.启动
```
[root@nick ~]# start-all.sh
starting namenode, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-namenode-nick.out
nick_2: starting datanode, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-datanode-nick_2.out
nick_1: starting datanode, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-datanode-nick_1.out
nick_3: starting datanode, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-datanode-nick_3.out
nick: starting secondarynamenode, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-secondarynamenode-nick.out
starting jobtracker, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-jobtracker-nick.out
nick_1: starting tasktracker, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-tasktracker-nick_1.out
nick_2: starting tasktracker, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-tasktracker-nick_2.out
nick_3: starting tasktracker, logging to /usr/app/hadoop_1/libexec/../logs/hadoop-root-tasktracker-nick_3.out
```
查看主节点进程

```
[root@nick ~]# jps
61327 JobTracker
61426 Jps
61093 NameNode
61244 SecondaryNameNode
```

查看各个节从节点进程

```
[root@nick_1 ~]# jps
3674 Jps
3473 DataNode
3551 TaskTracker

```

## 8.关闭

```
[root@nick ~]# stop-all.sh
stopping jobtracker
nick_1: stopping tasktracker
nick_3: stopping tasktracker
nick_2: stopping tasktracker
stopping namenode
nick_1: stopping datanode
nick_3: stopping datanode
nick_2: stopping datanode
nick: stopping secondarynamenode
```