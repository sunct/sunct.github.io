---
title: ' php优先级问题'
thumbnail: /assets/articleImg/2019/timg-php.jpeg
tags:
  - PHP
categories:
  - - PHP
date: 2019-03-27 18:47:11
---

|结合方向|运算符|附加信息|
|--|--|--|
|非结合|clone new|[clone](http://phpstudy.php.cn/php/92.html) 和 [new](http://phpstudy.php.cn/php/76.html#language.oop5.basic.new)|
|左|\[|[array()](http://phpstudy.php.cn/php/155.html)|
|非结合|++ --| [递增／递减运算符](http://phpstudy.php.cn/php/42.html)|
|非结合|~ - (int) (float) (string) (array) (object) (bool) @|[类型](http://phpstudy.php.cn/php/12.html)
|非结合|instanceof|[类型](http://phpstudy.php.cn/php/12.html)
|右结合|!|[逻辑操作符](http://phpstudy.php.cn/php/43.html)
|左|\* / %|[算术运算符](http://phpstudy.php.cn/php/36.html)
|左|\+ - .|[算术运算符](http://phpstudy.php.cn/php/36.html)和[字符串运算符](http://phpstudy.php.cn/php/44.html)
|左|<< >>|[位运算符](http://phpstudy.php.cn/php/38.html)
|非结合|< <= > >= <>|[比较运算符](http://phpstudy.php.cn/php/39.html)
|非结合|\== != === !==|[比较运算符](http://phpstudy.php.cn/php/39.html)
|左|&|[位运算符](http://phpstudy.php.cn/php/38.html)和引用
|左|^|[位运算符](http://phpstudy.php.cn/php/38.html)
|左|\| |[位运算符](http://phpstudy.php.cn/php/38.html)
|左|&&|[逻辑运算符](http://phpstudy.php.cn/php/43.html)
|左|&&|[逻辑运算符](http://phpstudy.php.cn/php/43.html)
|左|? :|[三元运算符](http://phpstudy.php.cn/php/39.html#language.operators.comparison.ternary)
|右|\= += -= \*= /= .= %= &= = ^= <<= >>=|[赋值运算符](http://phpstudy.php.cn/php/37.html)
|左|and|[逻辑运算符](http://phpstudy.php.cn/php/43.html)
|左|xor|[逻辑运算符](http://phpstudy.php.cn/php/43.html)
|左|or|[逻辑运算符](http://phpstudy.php.cn/php/43.html)
|左|,|多处用到左联表示表达式从左向右求值，右联相反

 <!--more--> 
举例：

```php
$a=1;  
$b=2;  
$c=3;  
if($a=4 || $b=5 && $c=6){  
    $a++;  
    $b++;  
}  
  
var_dump($a);  
var_dump($b);  
var_dump($c);
```
输出结果：

![](/assets/articleImg/2019/image-32-300x90.png)

> 说明：先运算 5&&$c ,结果是true，再运算 4 ,结果也是true， 后面的就不再执行，最后 再赋值运算$a=true; 进入if语句体进行运算，所以结果就是$a=true,$b=3,$c=3