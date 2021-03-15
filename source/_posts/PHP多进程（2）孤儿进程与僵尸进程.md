---
layout: post
title: PHP多进程（2）孤儿进程与僵尸进程
thumbnail: /assets/articleImg/2019/timg-php.jpeg
comments: true
tags:
  - PHP
categories:
  - - PHP
date: '2021-03-015 10:54:57'
abbrlink: 5a0a73c6
---

上一节：[PHP多进程（1）PHP多进程初探](/posts/547b869.html)，简单了解了一下关于PHP多进程和简单的通过代码了解其中的一些问题。

那这一节，来学习一下关于 `孤儿进程与僵尸进程`

本文参考地址：[https://github.com/elarity/advanced-php/blob/master/3.%20php%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%88%9D%E6%8E%A2---%E5%AD%A4%E5%84%BF%E5%92%8C%E5%83%B5%E5%B0%B8.md](https://github.com/elarity/advanced-php/blob/master/3.%20php%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%88%9D%E6%8E%A2---%E5%AD%A4%E5%84%BF%E5%92%8C%E5%83%B5%E5%B0%B8.md)

> 实际上，我们要记住：PHP的多进程是非常值得应用于生产环境具备高价值的生产力工具。


[上节](/posts/547b869.html)介绍的都是`pcntl_fork()`，只管fork生产，不管后续护理，实际上这样并不符合主流价值观，而且，操作系统本身资源有限，这样无限生产不顾护理，操作系统也会吃不消。

<!--more-->
`孤儿进程`是指父进程在fork出子进程后，子进程还在执行，但自己（父进程）先结束了。因此，子进程从此变得无依无靠、无家可归，变成了孤儿。用术语来表述就是，`父进程在子进程结束之前提前退出，这些子进程将由init（进程ID为1）进程收养并完成对其各种数据状态的收集。`

> init进程是Linux系统下的奇怪进程，这个进程是以普通用户权限运行但却具备超级权限的进程，简单地说，这个进程在Linux系统启动的时候做初始化工作，比如运行getty、比如会根据/etc/inittab中设置的运行等级初始化系统等等，当然了，还有一个作用就是如上所说的：收养孤儿进程。

`僵尸进程`是指父进程在fork出子进程，而后子进程在结束后，父进程并没有调用wait或者waitpid等完成对其清理善后工作，导致该子进程进程ID、文件描述符等依然保留在系统中，极大浪费了系统资源。所以，僵尸进程是对系统有危害的，而孤儿进程则相对来说没那么严重。在Linux系统中，我们可以通过`ps -aux`来查看进程，如果有`[Z+]`标记就是僵尸进程。

在PHP中，父进程对子进程的状态收集等是通过`pcntl_wait()`和`pcntl_waitpid()`等完成的，下面依然还是通过代码进行演示和说明。

演示并说明孤儿进程的出现，并演示孤儿进程被init进程收养：
```php

$pid = pcntl_fork();
if( $pid > 0 ){
    // 显示父进程的进程ID，这个函数可以是getmypid()，也可以用posix_getpid()
    echo "Father PID:".getmypid().PHP_EOL;
    // 让父进程停止两秒钟，在这两秒内，子进程的父进程ID还是这个父进程
    sleep( 2 );
} else if( 0 == $pid ) {
    // 让子进程循环10次，每次睡眠1s，然后每秒钟获取一次子进程的父进程进程ID
    for( $i = 1; $i <= 10; $i++ ){
        sleep( 1 );
        // posix_getppid()函数的作用就是获取当前进程的父进程进程ID
        echo '获取当前进程的父进程进程ID:'.posix_getppid().PHP_EOL;
    }
} else {
    echo "fork error.".PHP_EOL;
}
```


![演示孤儿进程结果](https://img-blog.csdnimg.cn/20210303163341753.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzNzc1MTY=,size_16,color_FFFFFF,t_70)
可以看到，输出的前部分（16:32:28），子进程的父进程进程ID为15828，但是从三秒后开始（16:32:31），由于父进程已经提前退出了，子进程变成孤儿进程，所以`init进程`收养了子进程，所以子进程的父进程进程ID变成了1。

演示并说明僵尸进程的出现，并演示僵尸进程的危害：
```php

$pid = pcntl_fork();
if( $pid > 0 ){
    // 下面这个函数可以更改php进程的名称
	cli_set_process_title('php father process');
    // 让主进程休息60秒钟
    sleep(60);
} else if( 0 == $pid ) {
    cli_set_process_title('php child process');
	// 让子进程休息20秒钟，但是进程结束后，父进程不对子进程做任何处理工作，这样这个子进程就会变成僵尸进程
	sleep(20);
} else {
    exit('fork error.'.PHP_EOL);
}
```

由于我用的是mac 电脑的php，导致出现⚠️警告错误:`PHP Warning:  cli_set_process_title(): cli_set_process_title had an error: Not initialized correctly in ... ...`。原因是：`mac os 不支持修改进程名称`。为此我改变一个运行环境来进行测试。

通过执行`ps -aux`命令可以看到，当程序在前20秒内运行的时候，php child process的状态列为[S+]，然而在20秒钟过后，这个状态变成了[Z+]，也就是变成了危害系统的僵尸进程。

运行结果如下图：
![子进程S+](https://img-blog.csdnimg.cn/20210303180455343.png)
![子进程Z+](https://img-blog.csdnimg.cn/20210303180529446.png)

那么，该如何避免出现类似上面的僵尸进程呢？PHP通过`pcntl_wait()`和`pcntl_waitpid()`两个函数来帮我们解决这个问题。

> 了解Linux系统编程的应该知道，看名字就知道这其实就是PHP把C语言中的wait()和waitpid()包装了一下。

通过`pcntl_wait()`来避免僵尸进程，在开始之前先简单普及一下`pcntl_wait()`的相关内容：
> `pcntl_wait()`这个函数的作用就是 “ 等待或者返回子进程的状态 ”，当父进程执行了该函数后，就会阻塞挂起等待子进程的状态一直等到子进程已经由于某种原因退出或者终止。换句话说就是如果子进程还没结束，那么父进程就会一直等，如果子进程已经结束，那么父进程就会立刻得到子进程状态。这个函数返回退出的子进程的进程ID或者失败返回-1。

> `pcntl_wait()`相关内容也可参考官网手册 [pcntl_wait](https://www.php.net/manual/zh/function.pcntl-wait.php)

我们将第二个案例（代码段）中代码修改一下：

```php
$pid = pcntl_fork();
if( $pid > 0 ){
    // 下面这个函数可以更改php进程的名称
    cli_set_process_title('php father process');

    // 返回$wait_result，就是子进程的进程号，如果子进程已经是僵尸进程则为0
    // 子进程状态则保存在了$status参数中，可以通过pcntl_wexitstatus()等一系列函数来查看$status的状态信息是什么
    $wait_result = pcntl_wait( $status );
    print_r( $wait_result );
    print_r( $status );

    // 让主进程休息60秒钟
    sleep(60);
} else if( 0 == $pid ) {
    cli_set_process_title('php child process');
    // 让子进程休息20秒钟，但是进程结束后，父进程不对子进程做任何处理工作，这样这个子进程就会变成僵尸进程
    sleep(20);
} else {
    exit('fork error.'.PHP_EOL);
}
```
在终端中通过ps -aux查看，可以看到在前20秒内，php child process是[S+]状态，然后20秒钟过后进程消失了，也就是被父进程回收了，没有变成僵尸进程。
![子进程S+](https://img-blog.csdnimg.cn/20210303181710464.png)
![子进程消失](https://img-blog.csdnimg.cn/20210303182049675.png)
但，`pcntl_wait()`也有阻塞的问题。

父进程只能挂起等待子进程结束或终止，在此期间父进程什么都不能做，这并不符合多快好省原则，所以`pcntl_waitpid()`闪亮登场。`pcntl_waitpid( $pid, &$status, $option = 0 )`的第三个参数如果设置为`WNOHANG`，那么父进程不会阻塞一直等待到有子进程退出或终止，否则将会和pcntl_wait()的表现类似。

修改第三个案例（代码块）的代码，但是，我们并`不添加` `WNOHANG`，演示说明`pcntl_waitpid()`功能：

```php
$pid = pcntl_fork();
if( $pid > 0 ){
    // 下面这个函数可以更改php进程的名称
	cli_set_process_title('php father process');
	
	// 返回值保存在$wait_result中
	// $pid参数表示 子进程的进程ID
	// 子进程状态则保存在了参数$status中
	// 将第三个option参数设置为常量WNOHANG，则可以避免主进程阻塞挂起，此处父进程将立即返回继续往下执行剩下的代码
	$wait_result = pcntl_waitpid( $pid, $status );
	var_dump( $wait_result );
	var_dump( $status );
	
    // 让主进程休息60秒钟
    sleep(60);
	
} else if( 0 == $pid ) {
    cli_set_process_title('php child process');
	// 让子进程休息20秒钟，但是进程结束后，父进程不对子进程做任何处理工作，这样这个子进程就会变成僵尸进程
	sleep(20);
} else {
    exit('fork error.'.PHP_EOL);
}
```
下面是运行结果，实际上可以看到主进程是被阻塞的，一直到第20秒子进程退出了，父进程不再阻塞：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303182737606.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303182803270.png)
接着修改第四段代码，添加第三个参数`WNOHANG`，代码如下：
```php
$pid = pcntl_fork();
if( $pid > 0 ){
    // 下面这个函数可以更改php进程的名称
    cli_set_process_title('php father process');

    // 返回值保存在$wait_result中
    // $pid参数表示 子进程的进程ID
    // 子进程状态则保存在了参数$status中
    // 将第三个option参数设置为常量WNOHANG，则可以避免主进程阻塞挂起，此处父进程将立即返回继续往下执行剩下的代码
    $wait_result = pcntl_waitpid( $pid, $status, WNOHANG );

    //$wait_result大于0代表子进程已退出,返回的是子进程的pid,非阻塞时0代表没取到退出子进程,为什么会没有取到子进程,因为当时子进程没有退出,在休眠sleep

    var_dump( $wait_result );
    var_dump( $status );
    echo "不阻塞，运行到这里".PHP_EOL;

    // 让主进程休息60秒钟
    sleep(60);

} else if( 0 == $pid ) {
    cli_set_process_title('php child process');
    // 让子进程休息20秒钟，但是进程结束后，父进程不对子进程做任何处理工作，这样这个子进程就会变成僵尸进程
    sleep(20);
} else {
    exit('fork error.'.PHP_EOL);
}
```

下面是运行结果，一个执行php程序的终端窗口，另一个是ps -aux终端窗口。可以看到主进程不再阻塞：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303183317615.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303183235966.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303183300673.png)



问题出现了，竟然php child process进程状态竟然变成了[Z+]，这是怎么搞得？回头分析一下代码：

我们看到子进程是睡眠了20秒钟，而父进程在执行`pcntl_waitpid()`之前没有任何睡眠且本身不再阻塞，所以，主进程自己先执行下去了，而子进程在足足20秒钟后才结束，进程状态自然无法得到回收。如果我们将代码修改一下，就是在主进程的`pcntl_waitpid()`前睡眠25秒钟，这样就可以回收子进程了。但是即便这样修改，细心想的话还是会有个问题，那就是在子进程结束后，在父进程执行`pcntl_waitpid()`回收前，有五秒钟的时间差，在这个时间差内，php child process也将会是僵尸进程。那么，`pcntl_waitpid()`如何正确使用啊？这样用，看起来毕竟不太科学。

那么，就需要引入`信号`。

下一节：[PHP多进程（3）信号](/posts/1c8636b0.html)

<br/>
<br/>
END
