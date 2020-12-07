---
title: ' 浏览器报错 net::ERR_CONTENT_LENGTH_MISMATCH 200 (OK) 解决办法'
tags: 浏览器
categories:
  - - 服务
date: 2019-10-16 16:50:37
---
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly90aW1nc2EuYmFpZHUuY29tL3RpbWc_aW1hZ2UmcXVhbGl0eT04MCZzaXplPWI5OTk5XzEwMDAwJnNlYz0xNTcxMjMwMzYzMjEyJmRpPTc3ZWUwOTNlNjRmN2EzYTk0ZGVlYTkxY2NjMzM3NDAzJmltZ3R5cGU9MCZzcmM9aHR0cCUzQSUyRiUyRmltZzMuZHVpdGFuZy5jb20lMkZ1cGxvYWRzJTJGaXRlbSUyRjIwMTYwNCUyRjI0JTJGMjAxNjA0MjQxNjMxMTBfUkszTlcudGh1bWIuNzAwXzAuanBlZw?x-oss-process=image/format,png)

<!--more--> 

## 一、发现问题

![](https://img-blog.csdnimg.cn/20191016163941630.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

​

在开发过程中，遇到前端页面加载css，js或woff，ttf  文件的时候，经常出现 `ERR_CONTENT_LENGTH_MISMATCH` 的报错情况。但不是所有的js或css 报错，报错的文件较没报错的文件偏大。并且报错的文件也可以单独在浏览器中打开,所以排除了最简单的地址错误。前端项目是由nginx代理的，所以可以查看nginx的日志，看看有无线索。  


## 二、解决问题

查找 nginx 错误日志文件

### 1、查找nginx配置文件

```linux
ps -ef  grep nginx
```

 结果如下：

```linux
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

> 我的开发环境nginx配置文件路径为： /usr/local/nginx/conf/nginx.conf

### 2、查看错误日志文件位置

```linux
cat /usr/local/nginx/conf/nginx.conf
```

![](https://img-blog.csdnimg.cn/20191016173836167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)



> 我的开发环境nginx错误日志路径为：cnk_data/nginx_logs/error.log

### 3、打开错误日志

根据出错的文件过滤一下（若文件过大，请稍等片刻）：

```linux
cat /cnk_data/nginx_logs/error.log  grep "query.dataTables.min.js"
```

结果如下：

```linux
2019/10/16 16:11:37 [crit] 5878#0: *81035 mkdir() "/usr/local/openresty/nginx/proxy_temp/2/35" failed (13: Permission denied) while reading upstream, client: 123.116.114.84, server: sunct.goapi.youlai.cn, request: "GET /static/js/adminone/jquery.dataTables.min.js HTTP/1.1", upstream: "http://127.0.0.1:9092/static/js/adminone/jquery.dataTables.min.js", host: "sunct.goapi.youlai.cn", referrer: "http://sunct.goapi.youlai.cn/admin/index"
```


其中有一句 mkdir()  Permission denied 错误：

> 2019/10/16 16:11:36 [crit] 5881#0: *80999 mkdir() `"/usr/local/openresty/nginx/proxy_temp/1/35"` failed (13: Permission denied) while reading upstream, client: 123.116.114.84, server: sunct.goapi.youlai.cn, request: "GET /static/js/adminone/jquery.dataTables.min.js HTTP/1.1", upstream: "http://127.0.0.1:9092/static/js/adminone/jquery.dataTables.min.js", host: "sunct.goapi.youlai.cn", referrer: "http://sunct.goapi.youlai.cn/admin/index"


到此，可以得知是没有`mkdir()`成功，结果因为没有权限，导致了请求失败，被拒绝。

> 那么，为什么nginx要访问`proxy_temp`文件夹呢？
> 
> 因为`proxy_temp`是nginx的缓存文件夹，我的css和js文件过大了，所以nginx一般会从缓存里面去拿，而不是每次都去原地址直接加载。

### 4、尝试解决

进入报错的路径，我的是  `/usr/local/openresty/nginx/`，查看文件夹`proxy_temp`权限。

```linux
cd /usr/local/openresty/nginx

ll
```

结果如下（注：这是改后的权限）

![](https://img-blog.csdnimg.cn/20191016175027866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)



根据个人情况，给`proxy_temp`文件夹重新修改权限和组 即可。

```linux
chown www root proxy_temp
chmod -Rf 777 proxy_temp
```

### 5、重启 nginx 服务

根据自己的nginx 服务配置来重启即可，命令可能如下：

```linux
./nginx -s reload
```
我使用的是：

```
/etc/init.d/nginx reload
```
## 6、重新刷新浏览器

完美，一切正常！

![](https://img-blog.csdnimg.cn/20191016180144985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20191016180103181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)



> 希望本文对你学习有所帮助，感谢您的阅读。