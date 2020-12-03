---
title: ' PHP写排序算法（二）快速排序'
tags: []
id: '536'
categories:
  - - 技术
---

#### 快速排序

**原理：**找到当前数组中的任意一个元素（一般选择第一个元素），作为标准，新建两个空数组，遍历整个数组元素，

如果遍历到的元素比当前的元素要小，那么就放到左边的数组，否则放到右面的数组，然后再对新数组进行同样的操作，

发现这里符合递归的原理，所以我们可以用递归来实现。

**使用递归，则需要找到递归点和递归出口：**

**递归点：如果数组的元素大于1，就需要再进行分解，所以我们的递归点就是新构造的数组元素个数大于1**

**递归出口：我们什么时候不需要再对新数组不进行排序了呢？就是当数组元素个数变成1的时候。**

第一次执行函数：  
26选为标尺，比26小的都放在左边数组 ，有1，4。其他的 76，43，41，86，45，49，71放在右边数组。  
第二次执行函数，  
上次的左边，1选为标尺，4放在右边数组，左边数组为空。  
第三次执行函数，  
上次的右边，76选为标尺，43，41，45，49，71放左边数组，86放右边数组。  
  
依次类推……  
  
  

$arr=\[26,76,43,41,86,1,45,49,71,4\];  
//快速排序  
**function** fast\_sort(**array** $arr) :**array**{  
    $len=count($arr);  
    // 递归出口  
    **if**($len<2){  
        **return** $arr;  
    }  
    // 选取一个标尺,选择第一个  
    $base\_arr\[0\]=$base=$arr\[0\];  
    // 初始化两个空数组  
    $left\_arr=$right\_arr=\[\];  
    **for** ($i=1;$i<$len;$i++){  
        **if**($arr\[$i\]<$base) {  
            $left\_arr\[\] = $arr\[$i\];  
        }**else**{  
            $right\_arr\[\]=$arr\[$i\];  
        }  
    }  
    var\_dump($left\_arr,$right\_arr);  
    **echo** '<hr/>';  
  
    // 左右数组递归  
    $left\_arr=fast\_sort($left\_arr);  
    $right\_arr=fast\_sort($right\_arr);  
  
    // 合并  
    **return** array\_merge($left\_arr,$base\_arr,$right\_arr);  
      
}  
  
var\_dump(fast\_sort($arr));

每次执行结果：

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-161-755x1024.png)

![](https://www.sunsanmiao.cn/wp-content/uploads/2019/04/image-162-727x1024.png)

时间复杂度：

稳定性：