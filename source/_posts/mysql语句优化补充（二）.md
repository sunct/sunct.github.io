---
title: ' MySQL语句优化补充（二）'
tags:
  - MySQL
  - 优化
id: '339'
categories:
  - - MySQL
date: 2019-04-06 18:42:08
---

除了增加索引外，还有很多注意的问题。

#### 1、不要在语句中出现计算

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-24.png)

#### 2、查询条件数据类型一致

为了测试把其中的一个订单id 改为 201903121704170005，看下图不同的查询：
 <!--more--> 
![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-26-1024x498.png)

显然oid=201903121704170005比 oid='201903121704170005'长很多,因为原来的oid 的字段类型是varchar，而不是数值型。分析一下：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-27-1024x680.png)

类型不一致的第一条，没有走索引。

#### 3、不要使用like左模糊查询

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-28-1024x258.png)

左模糊不走索引。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-29-1024x426.png)

右模糊不影响索引的使用

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-30-1024x597.png)

#### 4、select 避免使用\*查询所需字段

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-31-1024x850.png)

需要什么字段就查什么字段，避免使用\*。

#### 5、查询一条语句时加上limit 1

show profiles;

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-33-1024x92.png)

但是，并不是所有的查询加上limit 1，都能提高速度，我们再试一次：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-34-1024x189.png)

显然，前者加上limit 1 的查询时间反而比后者没加limit 1 花费更长的时间，即使差距只有0.00006s。那么在什么时候需要一条数据加上limit 1 的时候才能达到优化的效果呢？

请看下面的例子：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-35-1024x462.png)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-36-1024x507.png)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-37-1024x473.png)

通过简单的测试，当所选的记录id越靠前，使用limit 1 时，效果越明显。原因是，price 字段在没有加索引的情况下，需要进行全表扫描，加上limit 1,只要找到了对应的一条记录，就不会继续向下扫描了，效率会大大提高。 limit 1适用于查询结果为1条(也可能为0)会导致全表扫描的的SQL语句。

再看一下使用主键作为条件时，使用limit 1

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-38-1024x120.png)

从结果看来，效果不是特别明显，而时候反而加上limit 1，效率反而降低。原因是 sql 语句执行顺序为 select 查询完结果之后再进行 limit 1筛选。

所以，建议使用limit 1 最好是用在没加索引的where条件下。