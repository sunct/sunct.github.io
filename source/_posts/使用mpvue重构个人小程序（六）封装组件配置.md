---
title: ' 使用mpvue重构个人小程序（六）封装组件配置'
tags:
  - mpvue
  - 封装组件
  - 小程序
id: '417'
categories:
  - - 小程序
date: 2019-04-16 12:01:41
---

#### 自定义组件文件目录

在utils 中书写自己的公共方法

名字自己起，其中我的api.js文件中主要放的是和接口相关的文件：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-79-1024x280.png)

我的index.js 里放的是常用函数：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-80-1024x367.png)
 <!--more--> 
暴露给外部调用方法（函数）：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-83-1024x470.png)

#### 文件调用

和调用插件方法一样，写相对路径即可。

**import** util **from** '../../utils/index.js'

使用方法

**let** today = util.formatDate(**new** Date())

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-82-1024x347.png)