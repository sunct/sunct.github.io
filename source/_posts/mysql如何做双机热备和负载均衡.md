---
title: ' MySQL如何做双机热备和负载均衡'
tags:
  - MySQL
categories:
  - - MySQL
date: 2019-03-08 16:18:40
---

先简要介绍一下mysql双向热备：mysql从3.23.15版本以后提供数据库复制功能。利用该功能可以实现两个数据库同步，主从模式(A->B)，互相备份模式(A<=>B)的功能。

  
mysql数据库双向热备的操作实际说明：

  
## 1、mysql数据库同步复制功能的设置都在mysql的配置文件中体现。
 <!--more--> 
在linux环境下的配置文件一般在/etc/mysql/my.cnf或者在 mysql用户的home目录下的my.cnf，笔者的my.cnf则在/etc/my.cnf；windows环境下则可到mysql安装路径下找到 my.ini。mac的mamp环境下在tmp下找。

## 2、配置数据同步(A->B)（以mysql版本 5.0.26为例）：  
假设数据库A为主机：A机器：IP = 192.168.1.101 B机器：IP = 192.168.1.102  
### (1)在A机器中有数据库如下：  

```sql
-- 数据库A
CREATE DATABASE backup_db;
USE backup_db;
CREATE TABLE `backup_table` (
	`id` INT ( 11 ) NOT NULL auto_increment,
	`name` VARCHAR ( 20 ) CHARACTER 
	SET utf8 NOT NULL,
	`sex` VARCHAR ( 2 ) CHARACTER 
SET utf8 NOT NULL,
PRIMARY KEY ( `id` )) ENGINE = INNODB DEFAULT CHARSET = latin1;
```

A机器的my.cnf(或my.ini)中应该配置：  
server-id=1log-bin=c:\\mysqlback #同步事件的日志记录文件binlog-do-db=backup\_db #提供数据同步服务的数据库

### (2)在B机器中有数据库如下：  


```sql
-- 数据库B
CREATE DATABASE backup_db;
USE backup_db;
CREATE TABLE `backup\_table` (
	`id` INT ( 11 ) NOT NULL auto_increment,
	`name` VARCHAR ( 20 ) CHARACTER 
	SET utf8 NOT NULL,
	`sex` VARCHAR ( 2 ) CHARACTER 
SET utf8 NOT NULL,
PRIMARY KEY ( `id` )) ENGINE = INNODB DEFAULT CHARSET = latin1;
```
注：数据库A和B的数据库结构一定要相同，否则无法构成同步。  
B机器的my.cnf(或my.ini)中应该配置：  
```html
server-id=2 
master-host=192.168.1.101 #主机A的地址 
master-user=ym #主机A提供给B的用户,该用户中需要包括数据库backup_db的权限 
master-password=ym #访问密码 
master-port=3306 #端口，主机的MYSQL端口 
master-connect-retry=60 #重试间隔60秒 
replicate-do-db=backup_db #同步的数据库
```


### (3)完成了以上配置之后，将A的mysql数据的权限给B。  
A机器：
```linux
mysql>GRANT FILE ON *.* TO ym@’192.168.1.102′ IDENTIFIED BY ‘ym’;
```

### (4)重启AB数据库，后：

B机器：
```linux
mysql>slave start; //查看同步配置情况
```

A机器：
```linux
mysql>show master status;
```

B机器：
```linux
mysql>show slave status;
```

### (5)在A中的`backup_db.backup_table`表中插入一些数据，查看B中的`backup_db.backup_table`表是否同步了数据改动。如果没有看到同步数据结果，即同步不成功，请查看错误（如下）。  
当有错误产生时\*.err日志文件（可到mysql安装目录下找），同步的线程退出。当纠正错误后重复步骤(4)。

## 3、实现双向热备(A<=>B)：

  
将以上的(1)-(5)步骤按A-B双向配置即可。  
总结一下：  
主要是两边建立同样的数据库，然后在数据库配置文件里加入更新的语句即可。相互开通互有权限的用户，然后这条命令就是同步频率和同步数据库：
```html
master-connect-retry=60#重试间隔60秒 
replicate-do-db=backup_db #同步的数据库
```

## 4、另外，可能用到的代码语句

根据不同的环境可能会用到的一些语句如下，仅供参考

分配权限给从数据库

```linux
rant replication slave on *.* to ‘rm’@’192.168.1.102’ identified by ‘rm’;
```
通过 CHANGE MASTER TO 来设置从数据库
```linux
CHANGE MASTER TO MASTER_HOST=’192.168.1.101′, MASTER_PORT=3306,MASTER_USER=’r’m’,
MASTER_PASSWORD=’rm’ ,MASTER_LOG_FILE=’backup_db ‘,MASTER_LOG_POS=0;

```
