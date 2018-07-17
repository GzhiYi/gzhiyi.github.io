---
title: JavaScript隐式类型转化的问题
date: 2018-07-09 10:57:37
tags:
categories: JavaScript
---
### 前言
最近在jike看到一张图，是关于JavaScript隐式类型转换的。JavaScript这个语言的隐式转化会看起来很让人不可思议，这甚至也会出现在前端面试题，其实这就没必要了，个人觉得技术面试还是注重学习能力，经验和应对问题能力就行。解释不到位的化还是[万事通](https://stackoverflow.com)。  
那我就依据下面这种图趁机看看这有没好说的。    
![微信图片_20180709105621](http://p7b9iw239.bkt.clouddn.com/微信图片_20180709105621.png)
<!--more-->

### typeof NaN > "Number"
**NaN is a number but is not-a-number. Hum... what ?!**   
NaN意思"不是一个数字！"，那类型操作符typeof下为啥返回Number?
解释：NaN其实还是Number类型的，尽管他标称为不是Number。  
NaN意思就是一个特定的不能被限制的数字类型内表示的值。NaN is a special case.

### 99999999999999999 > 100000000000000000
一样适用于：0.1 + 0.2 不等于0.3
解释： 所有JavaScript数值都是双精度浮点数，
关于这部分说明，有兴趣细读：[JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)

### Math.max() < Math.min()
解释：Math.max的头参数为-Infinity，所以Math.max(4)实际为Math.max(-Infinity, 4)所以返回4，同理Math.min(4)也是返回4。
