---
title: ' 解决shell 里执行cd 的问题'
tags:
  - shell
id: '306'
categories:
  - - shell
date: 2019-04-02 21:43:15
---

在开发和学习的时候，经常要切换到固定的文件夹，于是写了个shell脚本用cd命令切换却发现目录切换不了。

代码如下：

 #!/bin/bash  
 cd /Applications/MAMP/htdocs/test/

> **解释：**执行的时候是./test\_dir.sh来执行的，这样执行的话终端会产生一个子shell（类似于C语言调用函数），子shell去执行我的脚本，在子shell中已经切换了目录了，但是子shell一旦执行完，马上退出，子shell中的变量和操作全部都收回。**回到终端根本就看不到这个过程的变化。**

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-1.png)

 <!--more--> 
验证一下：
```
#!/bin/bash  
history  
cd /Applications/MAMP/htdocs/test/  
sleep 1  
pwd
```

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-2.png)

首先按照 ./test\_dir.sh执行，这时候终端没有切换目录，history执行的结果是空的，说明子shell里面没有历史命令。

而pwd 输出结果： /Applications/MAMP/htdocs/test

解决方法：`source test_dir.sh`或者`. ./`test\_dir`.sh`

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image.png)