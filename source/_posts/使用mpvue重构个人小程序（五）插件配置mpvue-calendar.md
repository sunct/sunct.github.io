---
title: ' 使用mpvue重构个人小程序（五）插件配置mpvue-calendar'
tags:
  - mpvue
  - mpvue-calendar
  - 小程序
  - 插件
id: '410'
categories:
  - - 小程序
  - - 技术
date: 2019-04-16 11:49:31
---

#### 插件下载

根据项目需求，先加载日历组件，为了不重复造轮子，直接

```
npm i mpvue-calendar
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-63-1024x130.png)

或者

yarn add  mpvue-calendar

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-65-1024x317.png)

下载完成，在需要的页面进行插件引用
 <!--more--> 
**import** Calendar **from** 'mpvue-calendar'

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-73-1024x317.png)

components: {  
  Calendar  
},

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-74-1024x348.png)

具体的可参照文档，不在此一一说明，文档地址：[https://www.npmjs.com/package/mpvue-calendar](https://www.npmjs.com/package/mpvue-calendar)

当你引用完但是界面并不是源文档所说的那样，那么还需要进一步引用样式：

在src / main.js 中配置即可。

**import** 'mpvue-calendar/src/style.css'

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-75-1024x378.png)

插件原始样式：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-76-1024x977.png)

如果自己想修改样式，可以在自己文件中写style，设定为最高级 !importent

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-77-1024x587.png)

我现在使用的情况为：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-78-596x1024.png)