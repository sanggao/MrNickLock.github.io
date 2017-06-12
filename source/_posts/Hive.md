---
title: Hive
date: 2017-04-05 15:10:16
tags: 
- 学习笔记
- 大数据
- Hive
---

# hive 入门

## hive简介
- hive 是SQL解析引擎，将SQL转化为MR job 在hadoop执行
- hive表即 hdfs 的目录/文件夹（按照表名将文件夹分开）
	- 若为分区表，则分区值是子文件夹

## hive系统架构
- 用户接口
	- CLI 命令行客户端
	- JDBC/ODBC
	- WebUI

- 元数据存储 metastore 
	- derby（默认）
	- mysql 等

- 解释器、编译器、优化器、执行器
- hadoop：用hdfs进行存储，用MR进行计算

> 元数据 存于metastore
> （实际）数据存于 HDFS ：库表以文件夹形式，数据以文件形式

## mysql中元数据
- TBLS，表
- COLUMN_V2 字段
- SDS 位置

## 外部表

> 查看表结构
> ?select create table?

```sql
create external table
	stu 
	(stuno int,name string) 
	row format delimited fields terminated by '\t' 
	location '/stu'
```
## 分区表
```sql
create external table stu (int stuno,string name) row format delimited fields terminated by '\t' location '/stu'
```

