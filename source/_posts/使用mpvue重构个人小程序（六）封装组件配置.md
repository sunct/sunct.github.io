---
title: ' 使用mpvue重构个人小程序（六）封装组件配置'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
abbrlink: fdc1a9c5
date: 2019-04-16 12:01:41
---
上一节：[使用mpvue重构个人小程序（五）插件配置mpvue-calendar](/2019/04/16/使用mpvue重构个人小程序（五）插件配置mpvue-calendar/)

# 一、自定义组件文件目录

在`utils`中书写自己的公共方法

名字自己起，其中我的`api.js`文件中主要放的是和接口相关的文件：

![api.js](/assets/articleImg/2019/image-79-1024x280.png)
 <!--more--> 
我的`index.js`里放的是常用函数：

![index.js](/assets/articleImg/2019/image-80-1024x367.png)

暴露给外部调用方法（函数）：

![外部调用方法](/assets/articleImg/2019/image-83-1024x470.png)

# 二、文件调用

和调用插件方法一样，写相对路径即可。
```javascript
 import util from '../../utils/index.js'
```

使用方法
```javascript
 let today = util.formatDate(new Date())
```

![效果图](/assets/articleImg/2019/image-82-1024x347.png)

下一节：[使用mpvue重构个人小程序（七）使用自定义模板](/2019/04/16/使用mpvue重构个人小程序（七）使用自定义模板/)
