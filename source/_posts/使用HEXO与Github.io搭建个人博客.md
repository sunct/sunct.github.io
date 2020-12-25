---
layout: post
title: 使用HEXO与Github.io搭建个人博客
thumbnail: /assets/articleImg/2020/timg-hexo.jpeg
comments: true
toc: true
tags:
  - Hexo
abbrlink: 4081f3d5
date: 2020-05-09 20:16:57
---

 HEXO是基于Node.JS的一款简单快速的博客框架，能够支持多线程，支持markdown，可以将生成的静态网页发布到github.io以及coding上。
 
# 一、安装Node.js
关于Node.js的安装，在此就不再多费笔墨，没有安装的可以从官网和网上查找。下面只列举mac上安装。
<!--more-->
#### Mac OS 上安装

你可以通过以下两种方式在 Mac OS 上来安装 node：

- 1、在官方下载网站下载 pkg 安装包，直接点击安装即可。
- 2、使用 brew 命令来安装：
```linux
brew install node
```

# 二、安装hexo

运行以下命令
```linux
npm install -g hexo-cli
```
或者
```linux
npm install hexo-deployer-git –save
```
安装完之后检测一下：
```linux
hexo version
```
我的运行结果如下，可能和你的有些偏差：
```linux
hexo: 4.2.0
hexo-cli: 3.1.0
os: Darwin 18.7.0 darwin x64
http_parser: 2.8.0
node: 10.15.3
v8: 6.8.275.32-node.51
uv: 1.23.2
zlib: 1.2.11
ares: 1.15.0
modules: 64
nghttp2: 1.34.0
napi: 3
openssl: 1.1.0j
icu: 62.1
unicode: 11.0
cldr: 33.1
tz: 2018e

```

# 三、配置
hexo本地文件在安装插件的时候，最好都是用root身份执行，不然经常遇到一些问题

初始化本地路径

在本地创建一个文件夹hexo（名字自己定义），并初始化
```linux
cd /hexo && hexo init
```

生成node_modules
```linux
npm install
```

生成本地博客
```linux
hexo g
hexo server
```
项目目录如下：

![目录](/assets/articleImg/2020/WX20200509-165741@2x.png)


终端显示如下：
```linux
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```


在浏览器下输入[http://localhost:4000](http://localhost:4000)，成功的话就生成hexo的标准界面。


# 四、更换主题

只需在项目 themes 文件下，加入自己想要的主题即可。

现在我从网上找了一个，地址：[https://github.com/litten/hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)，可根据博主的说明进行设置。

主题的里配置在 `/themes/yilia/_config.yml`里设置。

在根目录下找到 `_config.yml` 文件，大约在96行，更换主题即可。

```linux
theme: yilia   ##主题，主要修改这个地方

```
更改完主题，再重新构建一下，运行以下命令即可：

```linux
hexo g
hexo server
```

# 五、部署到github

首先，在github中建立一个仓库（没有账户的先创建一个吧！ O(∩_∩)O哈哈~），仓库名字为 `XXX`.github.io,其中 `XXX` 是自定义的名字，后缀必须是`.github.io`。

创建完仓库，点击 `Setting`

![Setting](/assets/articleImg/2020/1589014297690.jpg)

然后进行一些设置。往下滚动，在`GitHub Pages`条目下，会显示可访问的地址。

![效果图](/assets/articleImg/2020/1589014797098.jpg)

根据以上配置完，只有把`public`文件下的代码就可以传到 刚才创建的 github仓库就可以了。

```linux
cd /public

git init
git add .
git commit -m “填写注释信息”
git remote add origin git@github.com:sunct/sunct.github.io.git #写自己的仓库地址
git push -u origin master

```

如果出现这个错误：

```linux
To github.com:sunct/sunct.github.io.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:sunct/sunct.github.io.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
![效果图](/assets/articleImg/2020/WX20200509-173025@2x.png)

是本地和github上有历史冲突，运行：
```linux
git pull origin master --allow-unrelated-histories
```

# 六、写博客

在 `/source/_posts/`目录下写自己的博客就可以了。
写完后再构建即可，可现在本地预览一下 [http://localhost:4000](http://localhost:4000)。把构建完的`public`文件夹上传到github就可以了。

也可以使用命令把 `pubic` 文件夹下的文件 同步到 远端`gh-pages`分支，以便维护使用。
```linux
git subtree push --prefix public origin  gh-pages
```
![效果图](/assets/articleImg/2020/1605591636896.jpg)


