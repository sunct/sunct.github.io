---
title: ' 由HTTP想到了什么'
thumbnail: /assets/articleImg/2019/img-http.jpg
tags:
  - HTTP
categories:
  - - 网络
abbrlink: 419fa51d
date: 2019-04-04 23:59:19
---

## 一、HTTP是什么？ Hyper Text Transfer Protocol 超文本传输协议。

默认端口：80；HTTPS默认端口：443。

## 二、HTTP 的请求方式：get，post，head，delete，put，move。get和post的区别。

## 三、HTTP 状态码： 100-199 客户端。200-299 请求成功。300-399已经移动的文件。400-499 客户端错误。500-599 服务器错误。
 <!--more--> 
200 ok ；  
301 永久重定向；  
403 禁止访问；  
404 没有找到资源；  
500 服务器错误，不能完成客户端请求。  
502 坏的网关，代理服务出了问题。  
503服务器不可用，超载、宕机；  
504 网关超时。

## 四、HTTP报文：

### 1、请求行（请求方法：get；url：https://www.sunsanmiao.cn；http协议，状态码等），

![](/assets/articleImg/2019/image-163.png)

### 2、请求头部（键值对，客户端把请求信息告诉服务器）

![](/assets/articleImg/2019/image-164-1024x220.png)

### 3、空行。

### 4、请求报文。（起始行：状态；响应头部；空行；响应报文主体）

![](/assets/articleImg/2019/image-165-1024x391.png)

### 五：URL：统一资源定位符。协议；IP或域名；主机资源地址（/index.html）。

## 六、URI：统一资源标识符。用于标识某一互联网资源名称的字符串，唯一标识某一信息资源。

## 七：TCP/IP 网络协议。

TCP和UDP的区别。

> 1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接  
2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付  
3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的  
UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）  
4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信  
5、TCP首部开销20字节;UDP的首部开销小，只有8个字节  
6、TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道

## 八、DNS解析过程。
本地LDNS，host-》全球DNS服务器-》一层层解析cn-sunsanmiao.cn->www.sunsanmiao.cn->把解析出的IP发给本地LDNS->把记录缓存

## 九：HTTP协议工作原理。
客户端输入地址-》DNS解析-》建立TCP连接（三次握手）-》发送报文-》服务器响应返回http报文-》关闭HTTP连接，关闭TCP连接=》浏览器显示内容到屏幕上。