---
title: ' 使用mpvue重构个人小程序（一）安装'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-13 15:10:50
---

## mpvue安装

### 全局安装 vue-cli

```linux
$ npm install --global vue-cli
```
 
### 创建一个基于 mpvue-quickstart 模板的新项目

```linux
$ vue init mpvue/mpvue-quickstart my-project
```

<!--more--> 
### 安装依赖 
```linux
$ cd my-project
$ npm install
```
 
### 启动构建
```linux
$ npm run dev
```
 
![](/assets/articleImg/2019/image-40-1024x158.png)

我的微信小程序项目都在WeChatProjects目录下，根据提示输入相对应的内容。如下图：

![](/assets/articleImg/2019/image-41-1024x407.png)

根据上图的提示，cd 到项目目录下，cd WeChatProjects/my-project，执行安装

![](/assets/articleImg/2019/image-42-1024x141.png)

![](/assets/articleImg/2019/image-43-1024x452.png)

接下来，你只需要启动[微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)，引入项目即可预览到你的第一个 `mpvue` 小程序

![](/assets/articleImg/2019/image-44-1024x823.png)

[使用mpvue重构个人小程序（二）文件结构](/2019/04/13/使用mpvue重构个人小程序（二）文件结构/)