---
title: ' Linux基本命令学习(二)-mkdir、rm、rmdir、mv、cp'
thumbnail: /assets/articleImg/2019/img-linux.jpg
tags:
  - Linux
categories:
  - - Linux
date: 2019-03-11 13:42:45
---

## 1、mkdir

mkdir命令用于建立名称为 dirName 之子目录。
```linux
mkdir [-p] dirName   其中： -p 确保目录名称存在，不存在的就建一个。
```

mkdir dir1 创建一个叫做 'dir1' 的目录'   
mkdir dir1 dir2 同时创建两个目录   
mkdir -p /tmp/dir1/dir2 创建一个目录树 
 <!--more--> 
![](/assets/articleImg/2019/image-15-1024x322.png)

## 2、rm 

rm命令用于删除一个文件或者目录。
```linux
rm [options] name...
```

*  -i 删除前逐一询问确认。
*  -f 即使原档案属性设为唯读，亦直接删除，无需逐一确认。
*  -r 将目录及以下之档案亦逐一删除。

>rm  test.txt  rm：是否删除 一般文件 "test.txt"? y     
rm  homework   rm: 无法删除目录"homework": 是一个目录     
rm  -r  homework   rm：是否删除 目录 "homework"? y 先用
rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容   
rm -rf dir1 dir2 同时删除两个目录及它们的内容 

先用 vi 命令创建文件

![](/assets/articleImg/2019/image-16.png)

![](/assets/articleImg/2019/image-17(1).png)

然后用 rm 命令删除

![](/assets/articleImg/2019/image-18(1).png)

## 3、rmdir

rmdir命令删除空的目录。
```linux
rmdir [-p] dirName
```

-p 是当子目录被删除后使它也成为空目录的话，则顺便一并删除。

先用mkdir 命令创建目录 BBB/Text

![](/assets/articleImg/2019/image-19.png)

![](/assets/articleImg/2019/image-20(1).png)

rmdir -p BBB/Text 在工作目录下的 BBB 目录中，删除名为 Text 的子目录。若 Text 删除后，BBB 目录成为空目录，则 BBB 亦予删除。

![](/assets/articleImg/2019/image-21-1024x73.png)

## 4、mv

mv命令用来为文件或目录改名、或将文件或目录移入其它位置。

|命令格式|运行结果|
|--|--|
|mv 文件名 文件名|将源文件名改为目标文件名
|mv 文件名 目录名|将文件移动到目标目录
|mv 目录名 目录名|目标目录已存在，将源目录 <br/>移动到目标目录；目标  <br/>目录不存在则改名
mv 目录名 文件名|出错

## 5、cp

cp命令主要用于复制文件或目录。

> cp file1 file2 复制一个文件   
cp dir/\* . 复制一个目录下的所有文件到当前工作目录   
cp -a /tmp/dir1 . 复制一个目录到当前工作目录   
cp -a dir1 dir2 复制一个目录 

*  -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
*  -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
*  -f：覆盖已经存在的目标文件而不给出提示。
*  -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
*  -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
*  -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
*  -l：不复制文件，只是生成链接文件。

使用指令"cp"将当前目录"`test/`"下的所有文件复制到新目录"`newtest`"下，输入如下命令：
```linux
$ cp –r test/ newtest
```
