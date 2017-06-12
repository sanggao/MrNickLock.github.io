---
title: Sqoop
date: 2017-04-06 19:39:13
tags:
- 学习笔记
- 大数据
- Sqoop
---

    大数据:spark(scala),storm,flink,hbase(redis,newSql),flume,kafka,HUE,ooize消息中间件,(metaQ,rabbitQ,kafka)
    平台(CM)

    java JVM调优
    人工智能AI
    数据挖掘,推荐系统(各种复杂的算法 线性回归,聚类,分类)
    离散数据,数据结构,统计学
    Spark sparkSql spark streaming spark mlib  mahout+pig
    用户画像

    java(设计模式,jvm调优,集合,多线程并发)大秒系统,hive(impala,tez，spark sql),mapreduce,hdfs
    linux

### 安装配置

1. 下载sqoop http://archive.cloudera.com/cdh5/cdh/5/(下载cdh版本地址)
cdh

2. 上传tar包，并解压
tar -zxvf xxx.tar -C ../softwores/
unzip xxx.zip

3. 配置sqoop-env.sh文件
http://sqoop.apache.org/docs/1.4.5/SqoopUserGuide.html（文档说明）
配置
`$SQOOP_HOEM/conf/sqoop-env.sh`
4. copy mysql驱动到
`$SQOOP_HOME/lib`目录下
5. 进行简单的测试

```bash
bin/sqoop list-databases \
--connect jdbc:mysql://hadoop:3306 \
--username root \
--password 123456


bin/sqoop list-tables \
--connect jdbc:mysql://hadoop:3306/mysql \
--username root \
--password 123456
```

### import|export  以hdfs为主体，以RDBMS(如mysql)为客体

#### import(mysql -> hdfs):
drop tables if exists customer;
CREATE TABLE customer (
  id tinyint(4) NOT NULL AUTO_INCREMENT,
  name varchar(255) DEFAULT NULL,
  passwd varchar(255) DEFAULT NULL,
  PRIMARY KEY (id)
);
INSERT INTO customer VALUES (1, 'admin', 'admin');
INSERT INTO customer VALUES (2, 'dehua', 'dehua');
INSERT INTO customer VALUES (3, 'system', '123456');
INSERT INTO customer VALUES (4, 'jack', 'ja123');
INSERT INTO customer VALUES (5, 'lol', 'user123');
INSERT INTO customer VALUES (6, 'helloword', 'leijun');
INSERT INTO customer VALUES (7, 'beijing', 'hqph');
INSERT INTO customer VALUES (8, 'wumengda', '123456');
INSERT INTO customer2 VALUES (9, 'canglaoshi', 'yamadei');

12,canglaoshi,111111
13,bolaoshi,123123
14,guanxige,123456


import
bin/sqoop import --help

 --table 指定表名
 --target-dir 指定导入路径
 --as-parquetfile  导入的存储类型
 -m,--num-mappers  指定map个数(在mysql创建表的时候,尽量添加主键)
 --fields-terminated-by 行之间的分隔符
 --columns <col,col,col...>  指定导入哪些字段
 -e,--query <statement>  查询结果集
 join
 -z,--compress (指定压缩类型)
 --compression-codec <codec>(具体是哪种压缩格式) snappy(lzo)
 压缩以及解压速度是很可观集群机器参差不齐 谷歌开源的压缩算法
 --incremental <import-type> (增量的导入)
 --direct(对导mysql的数据的优化)

 
(如果不指定目录，默认在当前用户目录下/user/root/ /user/用户名/)
(url,username,password,table)
目的：把mysql中customer表中的数据导入到hdfs之上
bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer 


(指定目录，并设置map数,map数就是sqoop并行的体现,调整map个数是一种sqoop job的优化方式)
bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer \
--delete-target-dir \
--target-dir /user/root/sqoop/import_customer \
--num-mappers 1(-m 2)

(指定存储到hdfs上的存储的格式)
bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer \
--target-dir /user/root/sqoop/txt_import \
--fields-terminated-by ',' \
--delete-target-dir \
-m 1 \
--as-textfile

bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer \
--target-dir /user/root/sqoop/parquet_import \
--fields-terminated-by ',' \
--delete-target-dir \
--num-mappers 2 \
--as-parquetfile 


(在hive中创建表)
drop table if exists hive_user_parquet; 
create table hive_user_parquet(
id int,
username string,
password string
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS parquet ;
load data inpath '/user/root/sqoop/parquet_import' into table hive_user_parquet;

drop table if exists hive_user_textfile ;
create table hive_user_textfile(
id int,
username string,
password string
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS textfile ;
load data inpath '/user/root/sqoop/txt_import' into table hive_user_textfile;

parquet格式为null((1.4.5的bug)1.4.6)


(指定导入的字段，为过滤字段)
bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer \
--target-dir /user/root/sqoop/imp_customer_column \
--num-mappers 1 \
--columns id,name

bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--query 'select id, name from customer' \
--target-dir /user/root/sqoop/imp_my_user_query \
--num-mappers 1


(在使用query关键字时，where条件必须加 '$CONDITIONS' )
16/11/05 18:46:03 ERROR tool.ImportTool: Encountered IOException running import job: java.io.IOException: Query [select id, account from customer] must contain '$CONDITIONS' in WHERE clause.
        at org.apache.sqoop.manager.ConnManager.getColumnTypes(ConnManager.java:300)
        at org.apache.sqoop.orm.ClassWriter.getColumnTypes(ClassWriter.java:1833)
        at org.apache.sqoop.orm.ClassWriter.generate(ClassWriter.java:1645)
        at org.apache.sqoop.tool.CodeGenTool.generateORM(CodeGenTool.java:96)
        at org.apache.sqoop.tool.ImportTool.importTable(ImportTool.java:478)
        at org.apache.sqoop.tool.ImportTool.run(ImportTool.java:605)
        at org.apache.sqoop.Sqoop.run(Sqoop.java:143)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:70)
        at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:179)
        at org.apache.sqoop.Sqoop.runTool(Sqoop.java:218)
        at org.apache.sqoop.Sqoop.runTool(Sqoop.java:227)
        at org.apache.sqoop.Sqoop.main(Sqoop.java:236)

bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--query 'select id, name from customer where $CONDITIONS' \
--target-dir /user/root/sqoop/imp_customer_query \
--delete-target-dir \
--num-mappers 1

(在sqoop中使用压缩)
--compress
--compression-codec


(导入到hdfs的数据时压缩过，然后在Hive里面创建表，将压缩过的数据给load进去)
bin/sqoop import \
--connect jdbc:mysql://hadoop:3306/test \
--username root \
--password 123456 \
--table customer \
--target-dir /user/root/sqoop/imp_my_snappy2 \
--delete-target-dir \
--num-mappers 2 \
--compress \
--compression-codec org.apache.hadoop.io.compress.SnappyCodec

drop table if exists customer_snappy2 ;
create table default.customer_snappy2(
id int,
username string,
password string
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' ;
load data inpath '/user/root/sqoop/imp_my_snappy2' into table customer_snappy2 ;
