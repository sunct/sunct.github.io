---
title: ' 常见的 PHP 设计模式3–单例模式（三私+一公）'
tags:
  - 模式
categories:
  - - PHP
date: 2019-03-26 20:59:11
---

## 单例模式

单例模式确保某个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

单例模式是一种常见的设计模式，在计算机系统中，线程池、缓存、日志对象、对话框、打印机、数据库操作、显卡的驱动程序常被设计成单例。

php只有懒汉式单例、没有饿汉式单例。
### 特点
单例模式有以下3个特点：

1．只能有一个实例。

2．必须自行创建这个实例。

3．必须给其他对象提供这一实例。

那么为什么要使用PHP单例模式？

PHP一个主要应用场合就是应用程序与数据库打交道的场景，在一个应用中会存在大量的数据库操作，针对数据库句柄连接数据库的行为，使用**单例模式可以避免大量的new操作**。因为每一次new操作都会消耗系统和内存的资源。

1. 为什么必须是静态的?因为静态成员属于类,并被类所有实例所共享   
2. 为什么必须是私有的?不允许外部直接访问,仅允许通过类方法控制方法  
3. 为什么要有初始值null,因为类内部访问接口需要检测实例的状态,判断是否需要实例化  
  
知识点：  
> 一、三私一公：  
①、私有静态属性，又来储存生成的唯一对象  
②、私有构造函数  
③、私有克隆函数，防止克隆——clone  
④、公共静态方法，用来访问静态属性储存的对象，如果没有对象，则生成此单例  
二、关键词instanceof  
检查此变量是否为该类的对象、子类、或是实现接口。

```php
<?php
/**
 *这是一个单例模式 demo
 *
 *此文件程序用来做什么的（详细说明，可选。）。
 * Created by PhpStorm
 * @author    sunct
 * @version   $Id$
 * @since     1.0
 */

class Mysql
{

    /**
     *
     * @var self[该属性用来保存实例] need
     */
    private static $instance;

    /**
     *
     * @var mixed
     */
    public $mix;

    /**
     * Return self instance[创建一个用来实例化对象的方法] need
     *
     * @return self
     */
    public static function getInstance()
    {
        if (! (self::$instance instanceof self)) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    /**
     * 构造函数为private,防止创建对象 need
     */
    private function __construct()
    {}

    /**
     * 防止对象被复制
     */
    private function __clone() need
    {
        trigger_error('Clone is not allowed !');
    }
}

// @test
$firstMysql = Mysql::getInstance();
$secondMysql = Mysql::getInstance();

$firstMysql->mix = "this_one<br/>";
$secondMysql->mix = "this_two<br/>";

print_r($firstMysql->mix);
// 输出： this_two
print_r($secondMysql->mix);
// 输出： this_two
```

输出结果：

![输出结果](/assets/articleImg/2019/image-31.png)

### 优点

一、实例控制单例模式会阻止其他对象实例化其自己的单例对象的副本，从而确保所有对象都访问唯一实例。

二、灵活性因为类控制了实例化过程，所以类可以灵活更改实例化过程。 

### 缺点

一、开销虽然数量很少，但如果每次对象请求引用时都要检查是否存在类的实例，将仍然需要一些开销。可以通过使用静态初始化解决此问题。

二、可能的开发混淆使用单例对象（尤其在类库中定义的对象）时，开发人员必须记住自己不能使用new关键字实例化对象。因为可能无法访问库源代码，因此应用程序开发人员可能会意外发现自己无法直接实例化此类。