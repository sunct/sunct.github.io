---
title: 爬虫|Charles（茶壶）工具在MAC端设置
thumbnail: /assets/articleImg/2019/timg-pachong.jpeg
tags:
  - Charles
  - 爬虫
categories:
  - - 服务
abbrlink: 48a81f3d
date: 2019-08-29 16:11:04
---

![Charles](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zczEuYmRzdGF0aWMuY29tLzcwY0Z2WFNoX1ExWW54R2twb1dLMUhGNmhoeS9pdC91PTM1Njg1NzQ2NDMsMjExNTIwMTEwNCZmbT0yNiZncD0wLmpwZw?x-oss-process=image/format,png)

## 一、说明和关于

Charles是一款代理服务器，通过设置将自己的电脑设备（服务器，浏览器）做为网络访问代理服务器，然后可以通过截取请求和发送请求，分析结果达到分析抓包的目的。
 <!--more--> 
 
本人系统环境为 mac ，如果有相关需要可联系我本人（联系方式可参照置顶文章 ：[获取我的联系方式](https://blog.csdn.net/u010377516/article/details/100122301)）获取。

> 声明：软件仅供个人开发学习使用，不可商用，如若有条件请 [购买正版](https://www.charlesproxy.com/) ，也可从官网获取使用免费版30天。 谢谢！

 
## 二、使用方法

目前我通过该软件主要应用于 爬虫抓包。

### 1、安装

通过安装包安装完成。不再描述。

需要说明的一点，如果在mac 环境安装 时出现 “app已损坏，打不开。你应该将它移到废纸篓。”

![app已损坏，打不开提示](https://img-blog.csdnimg.cn/20190829154134817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)
​

不要慌，这很可能是因为权限不足，打开 “系统偏好设置“——”安全与隐私“ 如下图所示

![系统偏好设置](https://img-blog.csdnimg.cn/20190829153503771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)


选择“任何来源”，如果没有这个选项，在终端，执行以下命令，并输入自己的密码。

```linux
sudo spctl --master-disable
```

然后就可以看到选中“任何来源”这个选项，然后重装软件即可。

![安全与隐私](https://img-blog.csdnimg.cn/20190829153544345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)


安装即可。

![安装提示](https://img-blog.csdnimg.cn/20190829153951332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)


### 2、代理设置

（1）

HTTP 代理端口设置为 ：8888

SOCKS 代理 启用把端口设置为 8889

其他全部勾选 - [x] ... 即可。如下图所示。

![](https://img-blog.csdnimg.cn/20190829144327737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)


（2）

在菜单中 找到“代理”并选择 “macOS Proxy”

![代理](https://img-blog.csdnimg.cn/20190829151038193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

​
(3)

如果还不能使用，设置根证书

在charles菜单中，选择“帮助” `help -> SSL Proxying -> Install Charles Root Certificate`，此时会打开mac的钥匙串访问程序，右键选择证书列表中的charles根证书，将该证书选择信任。

![帮助](https://img-blog.csdnimg.cn/20190829151354663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)

​
设置信任证书。

![设置信任证书](https://img-blog.csdnimg.cn/20190829151537556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)



> 另外注意，在使用软件时，请关闭其他代理。

使用时 点击 开启，圆点置红状态 即为可用。左边就会出现你请求的访问请求数据。

![开启](https://img-blog.csdnimg.cn/20190829151806681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)


然后就可以查看 返回的数据，包括参数和结果：

![查看](https://img-blog.csdnimg.cn/20190829154717639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)
