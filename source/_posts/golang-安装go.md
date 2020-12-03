---
title: ' golang - 安装go'
tags:
  - Golang
id: '649'
categories:
  - - Golang
date: 2020-01-15 16:03:50
---

## 安装go

*   [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#linux-%E7%8E%AF%E5%A2%83%E4%B8%8B%E5%AE%89%E8%A3%85)linux 环境下安装

安装包可从[go官网](https://golang.google.cn/dl/) 或者 [go 中文网](https://studygolang.com/dl) ，找到安装包路径。如图所示，根据自己需求下载。

[![image](https://github.com/sunct/learning-go/raw/master/images/install-1.png)](https://github.com/sunct/learning-go/blob/master/images/install-1.png)

在操作环境下，执行下面的命令进行下载。（截止时间：2020年01月14日14:14:59）

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#1%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85%E5%8C%85)1、下载安装包
 <!--more--> 
```
wget https://studygolang.com/dl/golang/go1.13.6.linux-amd64.tar.gz
```

根据你当前时间使用最新版本。

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#2%E6%8F%90%E5%8F%96%E5%8E%8B%E7%BC%A9%E5%8C%85)2、提取压缩包

提取压缩包到合适的目录（例如: /usr/local ，该目录一般是默认的目录），（注：根据自己的安装包修改命令中的版本号。）

```
sudo tar -xzf go1.13.6.linux-amd64.tar.gz -C /usr/local
```

解压完，可删除压缩包：

```
rm -rf go1.13.6.linux-amd64.tar.gz
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#3%E6%9F%A5%E7%9C%8B%E5%AE%89%E8%A3%85%E7%89%88%E6%9C%AC)3、查看安装版本

```
go version
```

### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#4%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)4、配置环境变量

```
vim /etc/profile
```

进入编辑界面后 Shift+G 跳转至尾行，按 o 新插入一行，输入如下增加或修改：

```
export GOROOT=/usr/local/go
export GOBIN=$GOROOT/bin
export PATH=$PATH:$GOBIN
```

保存，并执行

```
source /etc/profile
```

### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#5%E6%9F%A5%E7%9C%8B%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E4%BF%A1%E6%81%AF)5、查看环境变量信息

```
go env
```

信息如下：

```
GO111MODULE=""
GOARCH="amd64"
GOBIN="/usr/local/go/bin"
GOCACHE="/root/.cache/go-build"
GOENV="/root/.config/go/env"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="linux"
GONOPROXY=""
GONOSUMDB=""
GOOS="linux"
GOPATH="/root/go"
GOPRIVATE=""
GOPROXY="https://proxy.golang.org,direct"
GOROOT="/usr/local/go"
GOSUMDB="sum.golang.org"

```

### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#6go%E7%89%88%E6%9C%AC%E5%8D%87%E7%BA%A7)6、go版本升级

先删除旧版本，只需删除/usr/local/go 文件即可。运行命令：

```
sudo rm -rf /usr/local/go
```

再根据此文进行 下载新版本包并安装即可。

*   [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#mac-%E7%8E%AF%E5%A2%83%E4%B8%8B%E5%AE%89%E8%A3%85)mac 环境下安装

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#1%E5%AE%89%E8%A3%85)1、安装

使用 brew 运行命令：

```
brew install go
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#2%E6%9F%A5%E7%9C%8B%E5%AE%89%E8%A3%85%E7%89%88%E6%9C%AC)2、查看安装版本

```
go version
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#3%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)3、配置环境变量

编辑

```
vim ~/.bash_profile
```

可参考我的配置：

```
export GOROOT="/usr/local/Cellar/go/1.13.6/libexec"
export GOPATH=$HOME/code/go
export GOBIN=$GOPATH/bin
```

保存，并执行

```
source ~/.bash_profile
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#4%E5%8D%87%E7%BA%A7%E7%89%88%E6%9C%AC)4、升级版本

```
brew upgrade go
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#5%E5%88%87%E6%8D%A2%E7%89%88%E6%9C%AC)5、切换版本

```
brew switch go 1.13.6
```

#### [](https://github.com/sunct/learning-go/blob/master/learning-base/install.md#6%E5%8D%B8%E8%BD%BD)6、卸载

```
brew uninstall go
```