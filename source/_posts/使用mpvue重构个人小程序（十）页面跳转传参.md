---
title: ' 使用mpvue重构个人小程序（十）页面跳转传参'
tags:
  - mpvue
  - 小程序
  - 跳转传参
id: '463'
categories:
  - - 小程序
  - - 技术
date: 2019-04-16 14:22:21
---

前面已经使用到页面跳转，在此简单说明一下页面跳转和参数传递。

定义跳转方式，使用@click 触发。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-120-1024x357.png)

或者

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-121-1024x234.png)
 <!--more--> 
以第二个写留言为例，做跳转

定义方法并传递参数：

// 写留言  
comment (e) {  
  console.log(e)  
  **let** sid = e.currentTarget.id  
  console.log(sid)  
  wx.navigateTo({ url: '../comment/main?sid=' + sid })  
},

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-122-1024x353.png)

接收参数

在 comment.vue 的 onLoad 的 options 接收：

onLoad (options) {  
  console.log(options)  
  **let** islogin = **false  
  this**.sid = options.sid  
  
……

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-124-1024x279.png)

测试结果：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-125-1024x675.png)