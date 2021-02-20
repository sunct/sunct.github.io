---
layout: post
title: PHP判断两个数字是否是同符号
thumbnail: /assets/articleImg/2019/timg-php.jpeg
comments: true
tags:
  - PHP
categories:
  - - PHP
date: '2021-02-20 10:47:20'
abbrlink: d9cf7021
---

> 问题：写一个函数，判断给定的两个数字是否是符号相同的，不可以使用比较运算符或与0比较。
>
> 例如 `is_same_symbol(-1, 10) == false`;  `is_same_symbol(10,20)=true`;  `is_same_symbol(-1,-10)=true`; 同时，规定0属于正数。


> 在二进制表示中，最高位是1的话，就是负数。最高位为0则为正数。因此我可以想办法通过位运算来判断。1 ^0 = 1。所以 负数^正数=负数。其实就是类似于乘法了。

<!--more-->

```php

function is_same_symbol($a,$b){
	return (($a ^ $b) > 0);
}

$a=12;
$b=-10;

if (is_same_symbol($a, $b) == true){
    printf ("same symbol");
}else{
    printf ("not same symbol");
}
```
- 上面的例子 输出：`not same symbol`


> 这里用到了比较运算符(`^`)。其实完全可以再简化一下，把函数中  `>0` 的判断比较去掉，因为我们只需要知道第一位`符号位`即可。

```php

function is_same_symbol($a,$b){
	return !(($a ^ $b) >> 31);
}
```

> 右移31位，则只剩下最高位符号位，不是0，就是1。

> 另外，如果处理非数字型，而是字符串型的数字，如 `$a="100.01"`,`$b="-10.01"`，只需转化成数字型即可。