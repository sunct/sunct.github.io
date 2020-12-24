---
title: ' MySQL存储引擎MyISAM与InnoDB的区别比较'
thumbnail: /assets/articleImg/2019/timg-mysql.jpeg
tags:
  - MySQL
categories:
  - - MySQL
abbrlink: '10651370'
date: 2019-03-08 16:44:20
---

使用MySQL当然会接触到MySQL的存储引擎，在新建数据库和新建数据表的时候都会看到。

MySQL默认的存储引擎是MyISAM，其他常用的就是InnoDB了。至于到底用哪种存储引擎比较好？这个问题是没有定论的，需要根据你的需求和环境来衡量。所以对这两种引擎的概念、原理、异同和各自的优劣点有了详细的了解之后，再根据自己的情况选择起来就容易多了。
 <!--more--> 
|比较类型|MyISAM|InnoDB|
|--|--|
|存储结构|每张表被存放在三个文件：<br/>frm - 表格定义  <br/>MYD(MYData) - 数据文件  <br/>MYI(MYIndex) - 索引文件|所有的表都保存在同一个数据文件中（也可能是多个文件，或者是独立的表空间文件），InnoDB表的大小只受限于操作系统文件的大小，一般为2GB
|存储空间|MyISAM可被压缩，存储空间较小|InnoDB的表需要更多的内存和存储，它会在主内存中建立其专用的缓冲池，用于高速缓冲数据和索引
|可移植性、备份及恢复|由于MyISAM的数据是以文件的形式存储，所以在跨平台的数据转移中会很方便。在备份和恢复时可单独针对某个表进行操作|免费的方案可以是拷贝数据文件、备份 binlog，或者用 mysqldump，在数据量达到几十G的时候就相对痛苦了
|事务安全|不支持，每次查询具有原子性|支持 具有事务、回滚和崩溃修复能力(crash recovery capabilities)的事务安全(transaction-safe (ACID compliant))型表
AUTO_INCREMENT|MyISAM表可以和其他字段一起建立联合索引|InnoDB中必须包含只有该字段的索引
SELECT|MyISAM更优
INSERT||InnoDB更优
UPDATE||InnoDB更优
DELETE||InnoDB更优 它不会重新建立表，而是一行一行的删除
COUNT without WHERE|MyISAM更优。因为MyISAM保存了表的具体行数|InnoDB没有保存表的具体行数，需要逐行扫描统计，就很慢了
COUNT with WHERE|一样|一样，InnoDB也会锁表
锁|只支持表锁|支持表锁、行锁。行锁大幅度提高了多用户并发操作的新能。但是InnoDB的行锁，只是在WHERE的主键是有效的，非主键的WHERE都会锁全表的
外键|不支持|支持
FULLTEXT全文索引|支持|5.6+支持 可以通过使用Sphinx从InnoDB中获得全文索引，会慢一点

总的来说，MyISAM和InnoDB各有优劣，各有各的使用环境。但是，InnoDB的设计目标是处理大容量数据库系统，它的CPU利用率是其它基于磁盘的关系数据库引擎所不能比的。我觉得使用InnoDB可以应对更为复杂的情况，特别是对并发的处理要比MyISAM高效。同时结合memcache也可以缓存SELECT来减少SELECT查询，从而提高整体性能。

## MyISAM和InnoDB两者的应用场景：

1) MyISAM管理非事务表。它提供高速存储和检索，以及全文搜索能力。如果应用中需要执行大量的SELECT查询，那么MyISAM是更好的选择。

2) InnoDB用于事务处理应用程序，具有众多特性，包括ACID事务支持。如果应用中需要执行大量的INSERT或UPDATE操作，则应该使用InnoDB，这样可以提高多用户并发操作的性能。