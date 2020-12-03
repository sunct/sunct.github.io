---
title: ' 浏览器报错 net::ERR_CONTENT_LENGTH_MISMATCH 200 (OK) 解决办法'
tags: []
id: '665'
categories:
  - - 服务
date: 2019-10-16 16:50:37
---

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly90aW1nc2EuYmFpZHUuY29tL3RpbWc_aW1hZ2UmcXVhbGl0eT04MCZzaXplPWI5OTk5XzEwMDAwJnNlYz0xNTcxMjMwMzYzMjEyJmRpPTc3ZWUwOTNlNjRmN2EzYTk0ZGVlYTkxY2NjMzM3NDAzJmltZ3R5cGU9MCZzcmM9aHR0cCUzQSUyRiUyRmltZzMuZHVpdGFuZy5jb20lMkZ1cGxvYWRzJTJGaXRlbSUyRjIwMTYwNCUyRjI0JTJGMjAxNjA0MjQxNjMxMTBfUkszTlcudGh1bWIuNzAwXzAuanBlZw?x-oss-process=image/format,png)

​

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-12.gif)

# 一、发现问题

![](https://img-blog.csdnimg.cn/20191016163941630.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-15.gif)

​

在开发过程中，遇到前端页面加载css，js或woff，ttf  文件的时候，经常出现 `ERR_CONTENT_LENGTH_MISMATCH` 的报错情况。但不是所有的js或css 报错，报错的文件较没报错的文件偏大。并且报错的文件也可以单独在浏览器中打开,所以排除了最简单的地址错误。前端项目是由nginx代理的，所以可以查看nginx的日志，看看有无线索。  
<!--more--> 

# 二、解决问题

查找nginx 错误日志文件

## 1、查找nginx配置文件

```
ps -ef  grep nginx
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-13.gif)

 结果如下：

```
www      16951 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16952 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16953 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16954 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16955 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16956 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16957 18739  0 16:15 ?        00:00:00 nginx: worker process
www      16958 18739  0 16:15 ?        00:00:00 nginx: worker process
root     18739     1  0 Sep20 ?        00:00:26 nginx: master process /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-20.gif)

> 我的开发环境nginx配置文件路径为： /usr/local/nginx/conf/nginx.conf

## 2、查看错误日志文件位置

```
cat /usr/local/nginx/conf/nginx.conf
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-17.gif)

![](https://img-blog.csdnimg.cn/20191016173836167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-14.gif)

​

> 我的开发环境nginx错误日志路径为：cnk\_data/nginx\_logs/error.log

## 3、打开错误日志

根据出错的文件过滤一下（若文件过大，请稍等片刻）：

```
cat /cnk_data/nginx_logs/error.log  grep "query.dataTables.min.js"
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-18.gif)

 结果如下：

```
2019/10/16 16:11:37 [crit] 5878#0: *81035 mkdir() "/usr/local/openresty/nginx/proxy_temp/2/35" failed (13: Permission denied) while reading upstream, client: 123.116.114.84, server: sunct.goapi.youlai.cn, request: "GET /static/js/adminone/jquery.dataTables.min.js HTTP/1.1", upstream: "http://127.0.0.1:9092/static/js/adminone/jquery.dataTables.min.js", host: "sunct.goapi.youlai.cn", referrer: "http://sunct.goapi.youlai.cn/admin/index"
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-16.gif)

其中有一句 mkdir()  Permission denied 错误：

> 2019/10/16 16:11:36 \[crit\] 5881#0: \*80999 mkdir() "/usr/local/openresty/nginx/proxy\_temp/1/35" failed (13: Permission denied) while reading upstream, client: 123.116.114.84, server: sunct.goapi.youlai.cn, request: "GET /static/js/adminone/jquery.dataTables.min.js HTTP/1.1", upstream: "http://127.0.0.1:9092/static/js/adminone/jquery.dataTables.min.js", host: "sunct.goapi.youlai.cn", referrer: "http://sunct.goapi.youlai.cn/admin/index"

到此，可以得知是没有mkdir() 成功，结果因为没有权限，导致了请求失败，被拒绝。

> **那么，为什么nginx要访问`proxy_temp`文件夹呢？**
> 
> 因为`proxy_temp`是nginx的缓存文件夹，我的css和js文件过大了，所以nginx一般会从缓存里面去拿，而不是每次都去原地址直接加载。

## 4、尝试解决

进入报错的路径，我的是  /usr/local/openresty/nginx/，查看文件夹proxy\_temp 权限。

```
cd /usr/local/openresty/nginx

ll
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-21.gif)

结果如下（注：这是改后的权限）

![](https://img-blog.csdnimg.cn/20191016175027866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-19.gif)

​

根据个人情况，给proxy\_temp 文件夹重新修改权限和组 即可。

```
chown www root proxy_temp

chmod -Rf 777 proxy_temp
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-22.gif)

## 5、重启 nginx 服务

根据自己的nginx 服务配置来重启即可，命令可能如下：

```
./nginx -s reload
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-24.gif)

我使用的是：

```
/etc/init.d/nginx reload
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-23.gif)

## 6、重新刷新浏览器

完美，一切正常！

![](https://img-blog.csdnimg.cn/20191016180144985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-25.gif)

​

![](https://img-blog.csdnimg.cn/20191016180103181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://www.sunsanmiao.cn/wp-content/uploads/2020/03/image-26.gif)

​

> 希望本文对你学习有所帮助，感谢您的阅读。
> 
> 如果有相关需要可联系我本人（联系方式可参照置顶文章 ：[获取我的联系方式](https://blog.csdn.net/u010377516/article/details/100122301)）获取。