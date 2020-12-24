---
title: ' PHP防SQL注入'
thumbnail: /assets/articleImg/2019/timg-php.jpeg
tags:
  - SQL
  - PHP
categories:
  - - PHP
abbrlink: 33a07bb5
date: 2019-03-09 20:17:51
---

#### 什么是SQL注入攻击

[sql注入\_百度百科](http://www.baidu.com/link?url=_ld8xSkekqId1qpuoy9yGtMP1VC6Ot6TjNRHcdrs_HX2ngnBiMTLor3UDbG6Hjgp4fD3pQGbbDGcRCmSEXVcdK&wd=&eqid=8e1b83990078137c00000005574aa9ff)：

> 所谓SQL注入，就是通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到**欺骗服务器执行恶意的SQL命令**。具体来说，它是利用现有应用程序，将（恶意）的SQL命令注入到后台数据库引擎执行的能力，它可以通过在Web表单中输入（恶意）SQL语句得到一个存在安全漏洞的网站上的数据库，而不是按照设计者意图去执行SQL语句。

SQL注入攻击指的是通过构建特殊的输入作为参数传入Web应用程序，而这些输入大都是SQL语法里的一些组合，通过执行SQL语句进而执行攻击者所要的操作，其主要原因是程序没有细致地过滤用户输入的数据，致使非法数据侵入系统。
 <!--more--> 
#### **防范SQL注入攻击的方法：**

除了一些框架自带的过滤方法外，在php开发中，常用的函数有：

1、**addslashes($string):用反斜线引用字符串中的特殊字符' " \\**

$username=addslashes($username);

**2、htmlspecialchars($str);**此函数只转换5个字符，和号（&），双引号（“），单引号（‘），小于（<），大于（>），转换为实体形式，输出时浏览器会自动还原的，如果有意识的转换回来使用htmlspecialchars\_decode();

**3、mysql\_escape\_string($string)：用反斜杠转义字符串中的特殊字符，用于mysql\_query()查询。**

$username=mysql\_escape\_string($username);

**4、mysql\_real\_escape\_string($string)：转义SQL语句中使用的字符串中的特殊字符，并考虑到连接的当前字符集，需要保证当前是连接状态才能用该函数，否则会报警告。 不转义%与\_**

$username=mysql\_real\_escape\_string($username);

下面是一个封装防SQL注入函数，可参考。

mysql\_escape\_string(strip\_tags($arr\["$val"\])); /\*\* \* 函数名称：post\_check() \* 函数作用：对提交的编辑内容进行处理 \* 参　　数：$post: 要提交的内容 \* 返 回 值：$post: 返回过滤后的内容 \*/ function post\_check($post){ if(!get\_magic\_quotes\_gpc()){// 判断magic\_quotes\_gpc是否为打开 $post = addslashes($post);// 进行magic\_quotes\_gpc没有打开的情况对提交数据的过滤 } $post = str\_replace("\_","\\\_",$post);// 把 '\_'过滤掉 $post = str\_replace("%","\\%",$post);// 把 '%'过滤掉 $post = nl2br($post);// 回车转换 $post =htmlspecialchars($post);// html标记转换 return $post; }

注意：php.ini中有一个设置：magic\_quotes\_gpc = Off这个默认是关闭的，打开magic\_quotes\_gpc来防止SQL注入。

其他环境设置：

*   1、php.ini 中把safe\_mode 打开
*   2、safe\_mode\_gid = off
*   3、关闭危险函数 disable\_functions = chdir,chroot,dir,getcwd,opendir,readdir,scandir,fopen,unlink,delete,copy,mkdir, rmdir,rename,file,file\_get\_contents,fputs,fwrite,chgrp,chmod,chown
*   4、关闭PHP版本信息在http头中的泄漏  expose\_php = Off
*   5、打开magic\_quotes\_gpc来防止SQL注入  magic\_quotes\_gpc = On 这个默认是关闭的，如果它打开后将自动把用户提交对sql的查询进行转换 比如把 ' 转为 \\'等，这对防止sql注入有重大作用。
*   6、错误信息控制 error\_reporting = E\_WARNING & E\_ERROR 只显示警告以上
*   7、错误日志 建议在关闭display\_errors后能够把错误信息记录下来，便于查找服务器运行的原因 log\_errors = On