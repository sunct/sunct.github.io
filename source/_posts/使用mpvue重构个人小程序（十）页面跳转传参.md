---
title: ' 使用mpvue重构个人小程序（十）页面跳转传参'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-16 14:22:21
---
上一节：[使用mpvue重构个人小程序（九）下拉刷新](/2019/04/16/使用mpvue重构个人小程序（九）下拉刷新/)

前面已经使用到页面跳转，在此简单说明一下页面跳转和参数传递。

定义跳转方式，使用@click 触发。

![效果图](/assets/articleImg/2019/image-120-1024x357.png)
 <!--more--> 
或者

![效果图](/assets/articleImg/2019/image-121-1024x234.png)

以第二个写留言为例，做跳转

定义方法并传递参数：
```javascript
// 写留言  
comment (e) {  
  console.log(e)  
  **let** sid = e.currentTarget.id  
  console.log(sid)  
  wx.navigateTo({ url: '../comment/main?sid=' + sid })  
},
```

![效果图](/assets/articleImg/2019/image-122-1024x353.png)

接收参数

在 comment.vue 的 onLoad 的 options 接收：
```javascript
onLoad (options) {  
  console.log(options)  
  let islogin = false  
  this**.sid = options.sid
  
……

```

![效果图](/assets/articleImg/2019/image-124-1024x279.png)

测试结果：

![效果图](/assets/articleImg/2019/image-125-1024x675.png)