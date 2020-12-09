---
title: '使用mpvue重构个人小程序（八）使用全局变量'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-16 13:43:58
---
上一节：[使用mpvue重构个人小程序（七）使用自定义模板](/2019/04/16/使用mpvue重构个人小程序（七）使用自定义模板/)

在开发的过程中，用到全局变量的问题，搜了很多，方法五花八门，有的推荐使用vuex ，如果你想折腾一番可以自己尝试的使用。

下面，我用的是最原始的方式来介绍一下全局变量的使用方法。
 <!--more--> 
首先，在配置文件`src/mian.js` 中进行开启和全局配置。

![src/mian.js](/assets/articleImg/2019/image-100-1024x395.png)

我的配置文件内容：
```javascript
import Vue from ‘vue’
import App from ‘./App’
import ‘mpvue-calendar/src/style.css’
Vue.config.productionTip = *false
*App.mpType = ‘app’

const app = new Vue(App)
app.$mount()
getApp().globalData = {uid: ‘123456’}
Vue.prototype.globalData = getApp().globalData


app.$mount()  
getApp().globalData = {uid: '123456'}  
Vue.prototype.globalData = getApp().globalData

```

放到 `app.$mount()` 后面。

为了直观的配置，先给`getApp().globalData` 赋值。再给`Vue.prototype.globalData`，当然，你也可以直接赋值给`Vue.prototype.globalData`。效果一样。

![效果图](/assets/articleImg/2019/image-103-1024x484.png)

调用全局变量

使用 `this.globalData.uid` 调用即可。小程序重新编译：

执行过程

![效果图](/assets/articleImg/2019/image-107-1024x913.png)

![效果图](/assets/articleImg/2019/image-106-1024x437.png)

改变全局变量的值：

在写留言界面 改变值：`this.globalData.test` = '我已经改变了全局变量测试值'

![效果图](/assets/articleImg/2019/image-105-1024x398.png)

返回首页下拉刷新：

![效果图](/assets/articleImg/2019/image-108-1024x490.png)

改变成功！

下一节：[使用mpvue重构个人小程序（九）下拉刷新](/2019/04/16/使用mpvue重构个人小程序（九）下拉刷新/)
