---
title: ' php 中instanceof的使用'
thumbnail: /assets/articleImg/2019/timg-php.jpeg
tags:
  - PHP
categories:
  - - PHP
abbrlink: 9270c4ff
date: 2019-03-20 21:50:20
---

作用：

（1）判断一个对象是否是某个类的实例，

（2）判断一个对象是否实现了某个接口。

## 第一种用法：
```php
<?php   
$obj = new A();   
if ($obj instanceof A) {   
  echo 'A';   
}  
?>
```
 <!--more--> 
## 第二种用法：
```php
<?php   
interface ExampleInterface {   
   public function interfaceMethod();    
}    
class ExampleClass implements ExampleInterface {      
  public function interfaceMethod(){        
   return 'Hello World!';     
 }    
}   
$exampleInstance = new ExampleClass();    
if($exampleInstance instanceof ExampleInterface){      
  echo 'Yes, it is';    
}else{      
  echo 'No, it is not';   
}    
?>
```

输出结果：Yes, it is