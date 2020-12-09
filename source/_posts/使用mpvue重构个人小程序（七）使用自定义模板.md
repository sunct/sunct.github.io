---
title: ' 使用mpvue重构个人小程序（七）使用自定义模板'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-16 13:00:15
---

上一节：[使用mpvue重构个人小程序（六）封装组件配置](/2019/04/16/使用mpvue重构个人小程序（六）封装组件配置/)

# 一、模板文件

把自己的模板统一放在`src/components`目录下：

![模板文件](/assets/articleImg/2019/image-84-1024x350.png)
 <!--more--> 
 
# 二、调用模板
```javascript
import Sinfo from '@/components/sinfo'
```

![调用模板](/assets/articleImg/2019/image-85-1024x344.png)

把模板配置到组成部分`components`

名字和import 时的名字一致
```javascript
components: {  
  Sinfo  
},
```
使用组件
![使用组件](/assets/articleImg/2019/image-86-1024x273.png)

如果不使用会报错误

![抛出错误](/assets/articleImg/2019/image-88-1024x312.png)

页面代码中调用模板

![用模板](/assets/articleImg/2019/image-87-1024x404.png)

说明：模板标签不区分大小写 可以使用 sinfo 也可以使用 Sinfo

根据个人使用习惯和增加区分度，定义使用大写字母开头的标签。如图：

![效果图](/assets/articleImg/2019/image-89-1024x430.png)

其中 ：

:timeDate="timeDate" 是定义传给模板的参数。

ref="sinfo" 是模板回调使用。 和调用时使用的名字一致即可。

回调是当前文件调用模板里的方法（函数）使用。

```javascript
this.$refs.sinfo.getTodaySinfo(time, 1)
```

![效果图](/assets/articleImg/2019/image-90-1024x505.png)

模板函数：

![模板函数](/assets/articleImg/2019/image-91-1024x401.png)

如果名称不一致，则报 undefined 错误

![抛出错误](/assets/articleImg/2019/image-92-1024x202.png)

当我点选日历日期时，就可以调取模板中的方法，加载新的数据。

![效果图](/assets/articleImg/2019/image-96-1024x356.png)

![效果图](/assets/articleImg/2019/image-97-572x1024.png)

# 三、参数传递

为了测试，写死数据为 1111

![](/assets/articleImg/2019/image-94-1024x371.png)

接收参数：
```javascript
// 增加一个可从外部传入的属性  
props: {  
  timeDate: {  
    type: String,  
    default: []  
  }  
},

```


![效果图](/assets/articleImg/2019/image-95-1024x627.png)

小程序开发工具编译一下，在调试器中打印：

![调试](/assets/articleImg/2019/image-93-1024x368.png)

如何使用传递的参数默认加载模板数据呢？

在模板中，使用 mounted 或 onLoad 方法,调用自定义的函数。

![默认加载](/assets/articleImg/2019/image-99-1024x450.png)

为了更好的体现出 传递参数的作用，修改了一下 `getTodaySinfo( )`，在` mounted` 调用时就可以不用传递参数，没有参数默认是 `timeDate` 的值。

![默认参数](/assets/articleImg/2019/image-98-1024x542.png)

下一节：[使用mpvue重构个人小程序（八）使用全局变量](/2019/04/16/使用mpvue重构个人小程序（八）使用全局变量/)
