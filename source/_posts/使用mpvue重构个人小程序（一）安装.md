---
title: ' 使用mpvue重构个人小程序（一）安装'
tags:
  - mpvue
  - 小程序
id: '360'
categories:
  - - 小程序
  - - 技术
date: 2019-04-13 15:10:50
---

mpvue安装

_\# 全局安装 vue-cli_   
$ npm install --global vue-cli   
_\# 创建一个基于 mpvue-quickstart 模板的新项目_   
$ vue init mpvue/mpvue-quickstart my-project   
_\# 安装依赖_   
$ cd my-project   
$ npm install   
_\# 启动构建_   
$ npm run dev  

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-40-1024x158.png)

我的微信小程序项目都在WeChatProjects目录下，根据提示输入相对应的内容。如下图：
 <!--more--> 
![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-41-1024x407.png)

根据上图的提示，cd 到项目目录下，cd WeChatProjects/my-project，执行安装

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-42-1024x141.png)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-43-1024x452.png)

接下来，你只需要启动[微信开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/quickstart/basic/getting-started.html#%E5%AE%89%E8%A3%85%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7)，引入项目即可预览到你的第一个 `mpvue` 小程序

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-44-1024x823.png)

[使用mpvue重构个人小程序（二）文件结构](https://www.sunsanmiao.cn/archives/368)