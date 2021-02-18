---
layout: post
title: 基于github手动同步fork过的项目
thumbnail: /assets/articleImg/2021/github.png
comments: true
tags:
  - github
categories:
  - - 服务
date: '2021-01-118 11:11:20'
abbrlink: 14ebfabe
---

> 在 github中，发现好的项目然后fork放到自己的项目下。当发现原项目有更新时，自己也难免需要同步，此时就需要合并代码，接下来就介绍在github中，不需要命令来合并fork过的项目。

# 1、Pull request

> 打开自己的仓库，在【code】菜单下选择 ` Pull request`，或者直接选择  菜单中的` Pull request`

<!--more-->

![Pull request](/assets/articleImg/2021/WX20210218-113036@2x.png)

# 2、switching the base

> 点击下方的`switching the base`，改变同步方向。

![switching the base](/assets/articleImg/2021/1613619226716.jpg)

# 3、Create pull request

> 创建拉取请求。

![Create pull request](/assets/articleImg/2021/0A24ECB3-F5E3-4DB7-A5EC-93C9B0B79DD9.png)

 然后根据自己需求填写标题和内容说明，提交。

![Create pull request](/assets/articleImg/2021/1613619912734.jpg)


![Create pull request](/assets/articleImg/2021/1613620075061.jpg)


# 4、Merge pull request

> 把页面拉到最后，点击 `Merge pull request` 合并从源fork来的代码。

![Merge pull request](/assets/articleImg/2021/1613620335378.jpg)


![Merge pull request](/assets/articleImg/2021/1613625665129.jpg)


完成。

> 另外，如果不想如此麻烦操作，也可以把自己fork的项目删除先掉，然后再重新`fork`。

