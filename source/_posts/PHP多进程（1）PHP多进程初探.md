---
layout: post
title: PHP多进程（1）PHP多进程初探
thumbnail: /assets/articleImg/2019/timg-php.jpeg
comments: true
tags:
  - PHP
categories:
  - - PHP
date: '2021-03-015 10:45:57'
abbrlink: 547b869
---
## 问题描述：
> 近日在开发过程中出现了一个奇葩问题。 在我使用 PHP子进程处理发邮件的时候，在隔天再次1触发相关代码流程时，会把昨天的数据从使用子进程后再次重新处理一遍。导致数据出现重复，引发脏数据。为此，优化了代码，并且重新梳理了一下关于PHP多进程的问题。


本文参考地址：[https://github.com/elarity/advanced-php/blob/master/2.%20php%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%88%9D%E6%8E%A2---%E5%BC%80%E7%AF%87.md](https://github.com/elarity/advanced-php/blob/master/2.%20php%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%88%9D%E6%8E%A2---%E5%BC%80%E7%AF%87.md)
 
PHP是有多线程的。使用PHP的多线程首先需要下载安装一个线程安全版本（ZTS版本）的PHP，然后再安装pecl的pthread扩展。

实际上PHP是有多进程的，有一些人在用，总体来说php的多进程还算凑合，只需要在安装PHP的时候开启pcntl模块即可。在*NIX下，在终端命令行下使用php -m就可以看到是否开启了pcntl模块。
<!--more-->
现在只说php多进程问题。

> 注意：不要在apache或者fpm环境下使用php多进程，这将会产生不可预估的后果。



在php中我们使用`pcntl_fork()`来创建多进程（在*NIX系统的C语言编程中，已有进程通过调用fork函数来产生新的进程）。fork出来新进程则成为子进程，原进程则成为父进程，子进程拥有父进程的副本。这里要注意：


- 子进程与父进程共享程序正文段
- 子进程拥有父进程的数据空间和堆、栈的副本，注意是副本，不是共享
- 父进程和子进程将继续执行fork之后的程序代码
- fork之后，是父进程先执行还是子进程先执行无法确认，取决于系统调度（取决于信仰）

这里说子进程拥有父进程数据空间以及堆、栈的副本，实际上，在大多数的实现中也并不是真正的完全副本。更多是采用了COW（Copy On Write）即写时复制的技术来节约存储空间。简单来说，如果父进程和子进程都不修改这些 数据、堆、栈 的话，那么父进程和子进程则是暂时共享同一份 数据、堆、栈。只有当父进程或者子进程试图对 数据、堆、栈 进行修改的时候，才会产生复制操作，这就叫做写时复制。

在调用完`pcntl_fork()`后，该函数会返回两个值。在父进程中返回子进程的进程ID，在子进程内部本身返回数字0。由于多进程在apache或者fpm环境下无法正常运行，所以大家一定要在php cli环境下执行下面php代码。

代码执行前判断是否开启pcntl扩展
```php

if (!function_exists("pcntl_fork")) {
            die("pcntl extention is must !");
}
```

第一段代码，我们来说明在程序从`pcntl_fork()`后父进程和子进程将各自继续往下执行代码：
```php
    
    $pid = pcntl_fork();
	if( $pid > 0 ){
            echo "我是父亲".PHP_EOL;
	} else if( 0 == $pid ) {
            echo "我是儿子".PHP_EOL;
	} else {
            echo "fork失败".PHP_EOL;
    }
  ```
将文件保存为php_fork_test.php，然后使用cli执行，结果如下图所示：
![第一段代码执行结果](https://img-blog.csdnimg.cn/20210303155208868.png)

第二段代码，用来说明子进程拥有父进程的数据副本，而并不是共享：
```php

 // 初始化一个 number变量 数值为1

 $number = 1;
 $pid = pcntl_fork();
 if( $pid >0 ){
   $number += 1;
   echo "我是父亲，number 1 : { $number }".PHP_EOL;
 } else if( 0 == $pid ) {
   $number += 2;
   echo "我是父亲，number 2 : { $number }".PHP_EOL;
 } else {
   echo "fork失败".PHP_EOL;
 }
```
然后再使用cli执行，结果如下图所示：
![第二段代码执行结果](https://img-blog.csdnimg.cn/20210303155442962.png)

第三段代码，比较容易让人思维混乱，`pcntl_fork()`配合for循环来做些东西，问题：会显示几次 “儿子” ？

```php

 for( $i = 1; $i <= 3 ; $i++ ){
    $pid = pcntl_fork();
    if( $pid > 0 ){
       // do nothing ...
    } else if( 0 == $pid ){
        echo "儿子".PHP_EOL;
    }
}
```

上面代码执行结果如下： 
![第三段代码执行结果](https://img-blog.csdnimg.cn/20210303155621393.png)


仔细数数，竟然是显示了7次 “ 儿子 ”。好奇怪，难道不是3次吗？... ... 下面我修改一下代码，结合下面的代码，再思考一下为什么会产生7次而不是3次。

```php
for( $i = 1; $i <= 3 ; $i++){
     $pid = pcntl_fork();
     if( $pid >0 ){
        // do nothing ...
     } else if( 0 == $pid ){
         echo "儿子".PHP_EOL;
         exit;
     }
 }
```
执行结果如下图所示： 
![第三段代码执行结果](https://img-blog.csdnimg.cn/20210303155741379.png)


> 前面强调过：父进程和子进程将继续执行fork之后的程序代码(包含pcntl_fork函数)。

解释如下：

> i=1的时候父进程的pid不为0 这时候fork了一个pid=0的子进程a, 子进程数量1
> 
> i=2的时候父进程fork了一个子进程b, 子进程a又fork了一个子进程c, 子进程数量1+2
> 
> i=3的时候父进程fork了一个子进程d, a子进程fork了e, b子进程fork了f, c子进程fork了g子进程数量1+2+4=7

> 至于在fork子进程退出的时候 i=1 =2 =3的时候都只有一个父进程fork一个子进程 所以只有三个儿子



下一节：[PHP多进程（2）孤儿与僵尸进程](/posts/5a0a73c6.html)

<br/>
<br/>
END