---
title: ' php：布尔值（boolean）数据类型判断'
tags:
  - boolean
  - 布尔
id: '300'
categories:
  - - PHP
date: 2019-03-27 17:31:59
---

在PHP中，将变量明确转化为boolean值是可以使用(bool) 或者 (boolean) 来进行强制转化。

以下情况将变量转化为boolean时，值会为false

布尔值 且值为false  
整型值 0（零）  
浮点型值 0.0（零）  
空字符串  
字符串 “0”  
不包括任何元素的数组  
特殊类型 NULL（包括尚未赋值的变量  
从空标记生成的 SimpleXML 对象

变量值为字符串时，如果值为“”（即空）或0即为假，其它都为真（既使为0.00或“ ”中间有空格也是真）：

“  ”，“0.00”   都为 true