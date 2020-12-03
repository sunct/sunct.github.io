---
title: ' 使用mpvue重构个人小程序（四）全局配置'
tags:
  - mpvue
  - 小程序
  - 配置
id: '388'
categories:
  - - 小程序
  - - 技术
date: 2019-04-16 11:38:03
---

[使用mpvue重构个人小程序（三）使用Vue的语法编写](https://www.sunsanmiao.cn/archives/376)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-62-1024x774.png)

如何把右侧的小程序通过mpvue 进行重构呢？

为了干净，可以把pp.json里的 tabBar等数据清掉
 <!--more--> 
![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-68-1024x765.png)

只保留下图数据

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-67-1024x586.png)

#### 全局配置

在src / app.json 配置 全局属性：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-69-1024x463.png)

pages 是页面配置，加一个页面就配置一个。

window 是页面全局配置

"enablePullDownRefresh": **true**, // 开启下拉  
"backgroundTextStyle": "dark", // 头部背景主题颜色类型，包括 dark 和 light  
"navigationBarBackgroundColor": "#3F3F47",// 头部背景颜色  
"navigationBarTitleText": "雨豆",// 头部文本内容  
"navigationBarTextStyle": "white" // 头部文本颜色

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-70-1024x542.png)

#### 局部配置

在相关页面新建 main.json 文件

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-71-1024x708.png)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-72-1024x462.png)

是否支持下拉操作，也可以可以在单独页面配置，避免全局配置影响其他不需要下拉操作的页面。

其他配置在后续文章中陆续介绍。