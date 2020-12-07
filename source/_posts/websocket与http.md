---
title: ' WebSocket与HTTP'
tags:
  - HTTP
  - WebSocket
categories:
  - - PHP
date: 2019-03-20 21:06:07
---

![](https://img-blog.csdn.net/20170626140009906?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU0xfaWRlYXM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 一、相同点

1. 都是一样基于TCP的，都是可靠性传输协议。  
2. 都是应用层协议。

# 二、不同点

1. WebSocket是双向通信协议，模拟Socket协议，可以双向发送或接受信息。HTTP是单向的。http链接分为短链接，长链接，短链接是每次请求都要三次握手才能发送自己的信息。  
  
2. WebSocket是需要浏览器和服务器握手进行建立连接的。而http是浏览器发起向服务器的连接，服务器预先并不知道这个连接。  

# 三、联系

> WebSocket在建立握手时，数据是通过HTTP传输的。但是建立之后，在真正传输时候是不需要HTTP协议的。
