---
title: ' MySQL语句优化补充（一）'
tags:
  - MySQL
  - 优化
id: '316'
categories:
  - - MySQL
date: 2019-04-06 18:20:02
---

前面已经写了一些关于优化sql语句的方法。为了更形象的在项目中使用，以现有的数据再次详细说明一些，希望对sql优化有更进一步的学习。

#### 从发现问题到解决问题

以本地数据 my\_order表 为例，先生成119w多条记录。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-13.png)

根据id查询一条数据，用时0.00s
 <!--more--> 
![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-5.png)

用 explain 查看详情，用到了主键索引 primary key ；查询的行数row=1；

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-7-1024x503.png)

同理，若用oid 查询一条数据,用时0.26s

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-14-1024x269.png)

explain 查看一下,没用任何索引，查询的行数row=1186791

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-15-1024x439.png)

对比两条语句，差别在于where字句后的字段是否使用了索引，初步判断是因为索引的问题。先不说索引的问题。

#### 先开启慢查询

show variables like "slow%";  
  
set global slow\_query\_log=on;

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-10-1024x588.png)

设置慢查询时间，为了更好的测试设置为0.1s，在实际项目中根据需求设置，比如设为0.5s等。

show variables like "%long%";

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-11-1024x350.png)

set long\_query\_time=0.1;

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-12-1024x421.png)

再次运行 select \* from my\_order where oid='o\_201904060514268977';

打开慢日志文件，可查看刚才运行的语句。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-16-1024x265.png)

#### 查看性能

show variables like "%profiling%";

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-17.png)

set profiling=on;

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-18.png)

再次运行 select \* from my\_order where oid='o\_201904060514268977';

show profiles;  
//查看刚才运行的语句。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-19-1024x427.png)

可以看到刚才语句语句运行的更具体的时间。再进一步分析：

show profile for query 2;

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-20.png)

通过上图可以看到详细的运行时间。其中，Sending data花费时间最长。

并且，注意：logging slow query 也花费时间，一般不要在线上开启慢查询，等需要分析的时候再启用。

#### 加索引优化

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-21-1024x367.png)

增加索引，alter table my\_order add index(oid);

并运行：select \* from my\_order where oid='o\_201904060514268977';

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-22-1024x711.png)

再分析一下：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-23-1024x311.png)

看来加索引解决了刚才慢查询的问题。