---
title: ' Linux基本命令学习(四)-touch、chmod'
thumbnail: /assets/articleImg/2019/img-linux.jpg
tags:
  - Linux
categories:
  - - Linux
date: 2019-03-11 14:54:06
---

## 1、touch

touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。
```linux
touch [-acfm][-d<日期时间>][-r<参考文件或目录>] [-t<日期时间>][--help][--version][文件或目录…]

```

*   **参数说明**：
*   a 改变档案的读取时间记录。
*   m 改变档案的修改时间记录。
*   c 假如目的档案不存在，不会建立新的档案。与 --no-create 的效果一样。
*   f 不使用，是为了与其他 unix 系统的相容性而保留。
*   r 使用参考档的时间记录，与 --file 的效果一样。
*   d 设定时间与日期，可以使用各种不同的格式。
*   t 设定档案的时间记录，格式与 date 指令相同。
*   --no-create 不会建立新档案。
*   --help 列出指令格式。
*   --version 列出版本讯息。
 <!--more--> 
 ```linux
 $ touch testfile        #修改文件的时间属性 
 ```

使用指令"touch"时，如果指定的文件不存在，则将创建一个新的空白文件。例如，在当前目录下，使用该指令创建一个空白文件"file"，输入如下命令：
```linux
$ touch file            #创建一个名为“file”的新的空白文件 
```

## 2、chmod

chmod Linux/Unix 的文件调用权限分为三级 : 文件拥有者、群组、其他。利用 chmod 可以藉以控制文件如何被他人所调用。

```linux
chmod [-cfvR] [--help] [--version] mode file...
```


参数说明

mode : 权限设定字串，格式如下 :
```linux
[ugoa...][\[+-=][rwxX]...][,...]
```


其中：

*   u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
*   \+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
*   r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

其他参数说明：

*   \-c : 若该文件权限确实已经更改，才显示其更改动作
*   \-f : 若该文件权限无法被更改也不要显示错误讯息
*   \-v : 显示权限变更的详细资料
*   \-R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更)
*   \--help : 显示辅助说明
*   \--version : 显示版本

将文件 file1.txt 设为所有人皆可读取 :
```linux
chmod ugo+r file1.txt
```

将文件 file1.txt 设为所有人皆可读取 :
```linux
chmod a+r file1.txt
```

将文件 `file1.txt` 与 `file2.txt` 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 :

```linux
chmod ug+w,o-w file1.txt file2.txt
```

将 `ex1.py` 设定为只有该文件拥有者可以执行 :

```linux
chmod u+x ex1.py
```

将目前目录下的所有文件与子目录皆设为任何人可读取 :

```linux
chmod -R a+r \*

chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、写(w)和执行(x)的权限   
chmod go-rwx directory1 删除群组(g)与其他人(o)对目录的读写执行权限   
chown user1 file1 改变一个文件的所有人属性   
chown -R user1 directory1 改变一个目录的所有人属性并同时改变改目录下所有文件的属性
```