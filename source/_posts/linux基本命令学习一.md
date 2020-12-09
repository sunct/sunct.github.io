---
title: ' Linux基本命令学习(一)-ifconfig、cd、ls、vi、pwd'
thumbnail: /assets/articleImg/2019/img-linux.jpg
tags:
  - Linux
categories:
  - - Linux
date: 2019-03-11 13:00:09
---

## 1、ifconfig　　IP查询

![](/assets/articleImg/2019/image-2-1024x457.png)
 <!--more--> 
## 2、cd [目录名] 切换当前目录至dirName

cd /home 进入 '/ home' 目录'  
cd .. 返回上一级目录   
cd ../.. 返回上两级目录   
cd 进入个人的主目录   
cd ~user1 进入个人的主目录   
cd - 返回上次所在的目录 


## 3、ls [-option] 目录名称　　显示制定目录下的内容

![](/assets/articleImg/2019/image-3-1024x111.png)

　　-a　　显示所有文件和目录，包含隐藏文件和目录

![](/assets/articleImg/2019/image-4-1024x150.png)

　　-A　　显示所有文件和目录，包含隐藏文件和目录，但是不包含“.”,".."

![](/assets/articleImg/2019/image-5-1024x123.png)

　　-t　　根据时间排序

　　-l　　显示文件和目录的完整属性 等同于命令 ll

![](/assets/articleImg/2019/image-6-1024x867.png)

　　完整属性信息包含七部分：

　　第一部分：由10列组成，第一列（“d”表示目录，“l”表示链接文件，“-”表示普通文件）

　　　　　　   后九列，3个位一组，分为三组（“r”，read,可读；“w”,write,可写；“x”,execute,可执行）

　　　　　　　　　　第一组：文件拥有者所拥有的权限

　　　　　　　　　　第二组：文件拥有者所在群组成员所拥有的权限

　　　　　　　　　　第三组：其他人所拥有的权限

　　第二部分：节点，每增加一个硬链接，节点数加1

　　第三部分：拥有着

　　第四部分：群组（文件或者目录所在的群组）

　　第五部分：大小（单位：B）

　　第六部分：最新修改时间

　　第七部分：文件名

## 4、vi 文件名　如果vi后面的文件夹不存在，则新建文件夹；如果存在，则修改

　　vi详细步骤：

　　　　（1）vi 文件名（进入一般模式）

![](/assets/articleImg/2019/image-10-1024x54.png)

![](/assets/articleImg/2019/image-11-1024x498.png)

　　　　（2）按字母“i”(进入vi编辑模式，左下角出现insert)

![](/assets/articleImg/2019/image-12-1024x498.png)

　　　　（3）对文件进行编辑

　　　　（4）按“esc”（退出编辑模式，进入一般模式，左下角insert消失）

　　　　（5）输入：（进入命令模式）wq 保存退出；q! 强制退出不保存；w 文件另存为

![](/assets/articleImg/2019/image-13-1024x498.png)

## 5、pwd 显示工作路径

![](/assets/articleImg/2019/image-14.png)