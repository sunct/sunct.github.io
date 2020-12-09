---
title: ' 常见的 PHP 设计模式2–工厂模式（接口）'
thumbnail: /assets/articleImg/2019/timg-php.jpeg
tags:
  - 模式
categories:
  - - PHP
date: 2019-03-26 20:43:19
---
## 工厂模式

工厂模式是我们最常用的实例化对象模式，是用工厂方法代替new操作的一种模式。

使用工厂模式的好处是，如果你想要更改所实例化的类名等，则只需更改该工厂方法内容即可，不需逐一寻找代码中具体实例化的地方（new处）修改了。为系统结构提供灵活的动态扩展机制，减少了耦合。
 <!--more--> 
```php
<?php
 /**
  *这是一个工厂模式demo
  *
  *此文件程序用来做什么的（详细说明，可选。）。
  * Created by PhpStorm
  * @author    sunct
  * @version   $Id$
  * @since     1.0
  */
 
 /**
  *简单工厂模式（静态工厂方法模式）
  */
 /**
  * Interface people 人类
  */
 interface  people
 {
     public function  say();
 }
 /**
  * Class man 继承people的男人类
  */
 class man implements people
 {
     // 具体实现people的say方法
     public function say()
     {
         echo '我是男人<br>';
     }
 }
 /**
  * Class women 继承people的女人类
  */
 class women implements people
 {
     // 具体实现people的say方法
     public function say()
     {
         echo '我是女人<br>';
     }
 }
 /**
  * Class SimpleFactoty 工厂类
  */
 class SimpleFactoty
 {
     // 简单工厂里的静态方法-用于创建男人对象
     static function createMan()
     {
         return new man();
     }
     // 简单工厂里的静态方法-用于创建女人对象
     static function createWomen()
     {
         return new women();
     }
 }
 /**
  * 具体调用
  */
 $man = SimpleFactoty::createMan();
 $man->say();
 $woman = SimpleFactoty::createWomen();
 $woman->say();
 ```
输出结果：

![输出结果](/assets/articleImg/2019/image-30.png)