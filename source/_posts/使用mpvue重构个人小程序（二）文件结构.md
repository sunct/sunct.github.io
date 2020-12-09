---
title: ' 使用mpvue重构个人小程序（二）文件结构'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-13 15:36:33
---

上一节：[使用mpvue重构个人小程序（一）安装](/2019/04/13/使用mpvue重构个人小程序（一）安装/)

![文件结构](/assets/articleImg/2019/image-45-1024x633.png)

# package.json文件

package.json是项目的主配置文件，里面包含了mpvue项目的基本描述信息、项目所依赖的各种第三方库以及版本信息、以及可执行的脚本信息。
 <!--more--> 
![package.json](/assets/articleImg/2019/image-46-1024x710.png)

我们看到该文件中的`scripts`部分配置了4个可执行的命令：

```json
"dev": "node build/dev-server.js",
"start": "node build/dev-server.js",
"build": "node build/build.js",
"lint": "eslint --ext .js,.vue src"
```

`dev`和`start`是两个等价的命令，执行其中之一都可以将项目以开发模式启动。执行方式是：

```linux
npm start
npm run dev
```

`lint`指令是使用ESLint来进行代码语法和格式检查，以及修复一些可自动修复的问题。执行方式是：

```linux
npm run lint  #检查语法和格式
npm run lint -- --fix #检查代码语法和格式，并修复可自动修复的问题
```

`build`指令是用于生成发布用代码的，它会对代码进行一些压缩优化处理。当小程序开发完成后，将要提交审核时，请使用`build`来生成发布的代码。

# project.config.json文件

`project.config.json`文件是用于管理微信开发者工具的小程序项目的配置文件，其中记录了小程序的appid、代码主目录、以及编译选项等等信息，在微信开发者工具中导入小程序项目的时候主要是通过该配置文件读取和写入配置信息。

![project.config.json](/assets/articleImg/2019/image-47-1024x573.png)

# static目录

`static`目录可以用于存放各种小程序本地静态资源，如图片、文本文件等。代码中可通过相对路径或绝对路径进行访问。

![static](/assets/articleImg/2019/image-48-1024x635.png)

# build目录

`build`目录下是一些用于项目编译打包的node.js脚本和webpack配置文件。一般情况下不需要修改。

![build](/assets/articleImg/2019/image-49-1024x677.png)

# config目录

`config`目录下包含了用于开发和生产环境下的不同配置，`dev.env.js`用于开发环境，`prod.env.js`用于生产环境，可以将开发阶段和生产阶段不一样的信息（如后台API的url地址等）配置到这两个文件中去，然后在代码中以变量的形式进行引用。

![config](/assets/articleImg/2019/image-50-1024x584.png)

在编写请求后端API的代码时，你就可以使用这个环境配置：

```javascript
const baseURL = process.env.API_BASE_URL
wx.request({
  url: `${baseURL}/about`
})
```

这样一来，可以清楚的区分开发阶段和上线发布阶段的环境。

# src目录

`src`目录是我们主要进行小程序功能编写的地方。默认生成的demo代码为我们创建了几个子目录：`components`、`pages`和`utils`，还有2个文件：`App.vue`和`main.js`。其实它们都不是必须的，可以按照自己的风格进行定义和配置。不过默认创建的这个结构基本上是一个约定俗成的结构了，比较易于理解，所以我们可以遵循这个结构进行开发。

![src](/assets/articleImg/2019/image-51-1024x737.png)

*   `components`：在实际开发中，我们可以尽量将界面上可复用的部分，提取成vue组件放入该目录
*   `pages`：存放小程序的页面。请遵循每个小程序页面放入一个单独子目录的组织形式
*   `utils`：可选（可删）。可以将代码中一些公用工具函数组织成模块放入该目录下
*   可新建其他目录，存放你希望组织起来的代码。比如公用的业务逻辑代码、请求后台API的代码等等
*   `main.js + App.vue`：这两个是入口文件，相当于原生小程序框架中的`app.json`和`app.js`的复合体。

下一节：[使用mpvue重构个人小程序（三）使用Vue的语法编写](/2019/04/13/使用mpvue重构个人小程序（三）使用vue的语法编写/)
