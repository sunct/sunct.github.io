---
title: ' 使用mpvue重构个人小程序（九）下拉刷新'
tags:
  - mpvue
  - 下拉刷新
  - 小程序
id: '450'
categories:
  - - 小程序
  - - 技术
date: 2019-04-16 14:01:50
---

前面有几节已经提到过下拉刷新，本节，再详细的归结一下。

#### 配置

1、全局配置

"enablePullDownRefresh": **true**,

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-109-1024x488.png)

2、单独页面局部配置

没有main.json 自己新建一个即可。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-110-1024x555.png)

#### 使用

和methods 平级使用
 <!--more--> 
onPullDownRefresh () {  
  // to doing..  
  console.log('shuaxin')  
  // 停止下拉刷新  
  wx.stopPullDownRefresh()

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-111-1024x473.png)

模板中使用也是如此：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-112-1024x489.png)

**重新编译**，下拉刷新一下，结果：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-113-1024x734.png)

执行顺序， 当前页面 -> 模板页面。

我们用模板 sinfo.vue 来测试下拉加载。

onPullDownRefresh () {  
  console.log('下拉刷新1')  
  **let** that = **this  
** wx.showNavigationBarLoading() // 在标题栏中显示加载  
  // 模拟加载  
  setTimeout(**function** () {  
    // complete  
    that.getTodaySinfo()  
    wx.hideNavigationBarLoading() // 完成停止加载  
    wx.stopPullDownRefresh() // 停止下拉刷新  
  }, 1500)  
},

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-118-1024x421.png)

先去写留言，再返回首页，下拉加载。

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-114-1024x520.png)

返点左上角返回是不会更新的：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-115-1024x643.png)

下拉一下：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-116-1024x493.png)

下拉松手

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-117-1024x883.png)

完美！