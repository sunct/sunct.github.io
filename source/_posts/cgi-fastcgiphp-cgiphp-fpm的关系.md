---
title: ' CGI, FastCGI,PHP-CGI,php-fpm的关系'
thumbnail: /assets/articleImg/2019/img-linux.jpg
tags:
  - cgi
  - php-fpm
categories:
  - - Linux
abbrlink: cfdcbe73
date: 2019-04-04 23:51:44
---

网上搜了一大堆，说的都是云里雾里。于是自己整理了一下。

# 一、什么是CGI

CGI(Common Gateway Interface) 通用网关接口，CGI是外部应用程序（CGI程序）与WEB服务器之间的接口标准，是在CGI程序和Web服务器之间传递信息的过程。

![image](http://note.youdao.com/yws/api/personal/file/WEB6930b9d20e1c7361de390efa7e0b04d9?method=download&shareKey=ebd561eb88641be368153c6c34daa849)
 <!--more-->
Web服务器一般只处理静态文件请求（如 jpg、htm、html），如果碰到一个动态脚本请求（如php），服务器并不能直接运行php这样的文件，自己不能做，外包给别人吧，但是要与php做个约定，我给你什么，然后你给我什么。那这个约定就就是`CGI协议`。而实现这个协议的脚本叫做`CGI程序。`

web服务器主进程fork出一个新的进程来启动CGI程序，启动CGI程序需要一个过程，比如，读取配置文件，加载扩展等。CGI程序启动后，就会解析动态脚本，然后将结果返回给Web服务器，最后Web服务器再将结果返回给客户端，刚才fork的进程也会随之关闭。  
这样，每次用户请求动态脚本，Web服务器都要重新fork一个新进程，去启动CGI程序，由CGI程序来处理动态脚本，处理完后进程随之关闭。  
这种工作方式的效率是非常低下的。
 
# 二、什么是FastCGI

当请求量过大时CGI程序会严重浪费系统资源的。这样FastCGI就是为了解决这个问题。

FastCGI程序本身监听某个socket然后等待来自web服务器的连接，而不是像CGI程序是由web服务器 fork-exec，所以FastCGI本身是一个服务端程序，而web服务器对它来说则是客户端。

![image](http://note.youdao.com/yws/api/personal/file/WEB683ab0ce6daa0f9103971b05b3f4f5a4?method=download&shareKey=439b6e0fc69fd7f15815c6bce0960a9a)

> `FastCGI被设计用来支持常驻（long-lived）应用进程，减少了fork-and-execute带来的开销`
> 
> `FastCGI进程通过监听的socket，收来自Web服务器的连接，这样FastCGI进程可以独立部署`
> 
> `服务器和FastCGI监听的socket之间按照消息的形式发送环境变量和其他数据`

FastCGI程序和web服务器之间通过可靠的流式传输（Unix Domain Socket或TCP）来通信，相对于传统的CGI程序，有环境变量和标准输入输出，而FastCGI程序和web服务器之间则只有一条socket连接来传输数据，所以它把数据分成以下多种消息类型：

FastCGI会提供这样的功能：首先会由某个程序读取相应的配置文件并初始化执行环境，当这一系列步骤完成之后，他会一下生成很多个cgi进程（也就是进程池），这样在以后处理php的请求时就不需要频繁的“读取配置、创建进程、销毁进程这样的步骤了”，所以FastCGI可以理解为就是为了实现这种效果而产生的一种处理办法。

# 三、什么是PHP-CGI

PHP-CGI只是解释PHP脚本的程序。PHP-CGI只是个CGI程序，他自己本身只能解析请求，返回结果，不会进程管理。所以就出现了一些能够调度PHP-CGI进程的程序。

# 四、什么是php-fpm

用来实现FastCGI的操作。“php-fpm是FastCGI进程的管理器，用来管理FastCGI进程的”，这句话可以理解成php-fpm就是能够实现FastCGI功能的程序，他目前由php官方集成到php内核中。所以就是如果要实现cgi的进程池功能就需要使用php-fpm 。php-fpm除了进程管理外，还实现FastCGI协议并且支持进程平滑重启。

# 五、FastCGI 与 PHP-CGI关系

FastCGI是协议，在php中由php-fpm实现，管理着解析php脚本的进程，PHP-CGI是一个程序，专门处理php脚本。

参考：[http://yongxiong.leanote.com/post/%E4%BB%8ECGI%E5%88%B0FastCGI%E5%88%B0PHP-FPM](http://yongxiong.leanote.com/post/%E4%BB%8ECGI%E5%88%B0FastCGI%E5%88%B0PHP-FPM)