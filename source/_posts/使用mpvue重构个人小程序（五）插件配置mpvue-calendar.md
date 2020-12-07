---
title: ' 使用mpvue重构个人小程序（五）插件配置mpvue-calendar'
tags:
  - mpvue
  - mpvue-calendar
  - 小程序
categories:
  - - 小程序
date: 2019-04-16 11:49:31
---

上一节：[使用mpvue重构个人小程序（四）全局配置](/2019/04/16/使用mpvue重构个人小程序（四）全局配置/)

# 一、 插件下载

根据项目需求，先加载日历组件，为了不重复造轮子，直接安装。

```linux
npm i mpvue-calendar
```

![nmp方式](/assets/articleImg/2019/image-63-1024x130.png)
 <!--more--> 
或者
```linux
yarn add  mpvue-calendar
```
![yarn方式](/assets/articleImg/2019/image-65-1024x317.png)

下载完成，在需要的页面进行插件引用

# 二、插件引用
```javascript
import Calendar from 'mpvue-calendar'
```

![插件引用](/assets/articleImg/2019/image-73-1024x317.png)
添加为组件
```javascript
components: {  
  Calendar  
},

//其他代码
... ...


```
![添加为组件](/assets/articleImg/2019/image-74-1024x348.png)

具体的可参照文档，不在此一一说明，文档地址：[https://www.npmjs.com/package/mpvue-calendar](https://www.npmjs.com/package/mpvue-calendar)

# 三、样式修改

当你引用完但是界面并不是源文档所说的那样，那么还需要进一步引用样式：

在`src/main.js`中配置即可。

```javascript
import 'mpvue-calendar/src/style.css'
```

![引用样式](/assets/articleImg/2019/image-75-1024x378.png)

插件原始样式：

![原始样式](/assets/articleImg/2019/image-76-1024x977.png)

如果自己想修改样式，可以在自己文件中写style，设定为最高级 `!importent`

![日历样式](/assets/articleImg/2019/image-77-1024x587.png)

我现在使用的情况为：

![效果图](/assets/articleImg/2019/image-78-596x1024.png)

下一节：[使用mpvue重构个人小程序（六）封装组件配置](/2019/04/16/使用mpvue重构个人小程序（六）封装组件配置/)
