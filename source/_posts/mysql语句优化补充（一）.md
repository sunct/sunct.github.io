---
title: ' MySQL语句优化补充（一）'
thumbnail: /assets/articleImg/2019/timg-mysql.jpeg
tags:
  - MySQL
categories:
  - - MySQL
date: 2019-04-06 18:20:02
---

前面已经写了一些关于优化sql语句的方法。为了更形象的在项目中使用，以现有的数据再次详细说明一些，希望对sql优化有更进一步的学习。

## 一、从发现问题到解决问题

以本地数据 `my_order`表为例，先生成119w多条记录。

![](/assets/articleImg/2019/image-13.png)

根据id查询一条数据，用时0.00s
 <!--more--> 
![](/assets/articleImg/2019/image-5.png)

用 `explain` 查看详情，用到了主键索引 `primary key` ；查询的行数row=1；

![](/assets/articleImg/2019/image-7-1024x503.png)

同理，若用oid 查询一条数据,用时0.26s

![](/assets/articleImg/2019/image-14-1024x269.png)

explain 查看一下,没用任何索引，查询的行数row=1186791

![](/assets/articleImg/2019/image-15-1024x439.png)

对比两条语句，差别在于where字句后的字段是否使用了索引，初步判断是因为索引的问题。先不说索引的问题。

## 二、先开启慢查询
```sql
show variables like "slow%";  
  
set global slow_query_log=on;
```

![](/assets/articleImg/2019/image-10-1024x588.png)

设置慢查询时间，为了更好的测试设置为0.1s，在实际项目中根据需求设置，比如设为0.5s等。
```sql
show variables like "%long%";
```

![](/assets/articleImg/2019/image-11-1024x350.png)
```sql
set long_query_time=0.1;
```

![](/assets/articleImg/2019/image-12-1024x421.png)

再次运行 
```sql
select * from my_order where oid='o_201904060514268977';
```

打开慢日志文件，可查看刚才运行的语句。

![](/assets/articleImg/2019/image-16-1024x265.png)

## 三、查看性能
```sql
show variables like "%profiling%";
```

![](/assets/articleImg/2019/image-17.png)
```sql
set profiling=on;
```

![](/assets/articleImg/2019/image-18.png)

再次运行
```sql
select * from my\_order where oid='o_201904060514268977';
```
```sql
show profiles; 
```
![](/assets/articleImg/2019/image-19-1024x427.png)

可以看到刚才语句语句运行的更具体的时间。再进一步分析：

```sql
show profile for query 2;
```

![](/assets/articleImg/2019/image-20.png)

通过上图可以看到详细的运行时间。其中，Sending data花费时间最长。

并且，注意：logging slow query 也花费时间，一般不要在线上开启慢查询，等需要分析的时候再启用。

## 四、加索引优化

![](/assets/articleImg/2019/image-21-1024x367.png)

增加索引，
```sql
alter table my_order add index(oid);
```

并运行：

```sql
select * from my_order where oid='o_201904060514268977';
```

![](/assets/articleImg/2019/image-22-1024x711.png)

再分析一下：

![](/assets/articleImg/2019/image-23-1024x311.png)

看来加索引解决了刚才慢查询的问题。