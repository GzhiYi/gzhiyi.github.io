---
title: Linux搜狗输入法出现候选词乱码的问题
date: 2018-05-05 22:52:06
categories: "Linux"
---
### 搜狗输入法  
在Linux下，比较符合中文输入习惯的还是搜狗输入法。虽然还是有一些问题，但能为Linux提供一个友好的中文输入就很不错了。  
不仅仅是在Linux mint，之前用Ubuntu 16.04的时候也出现过搜狗输入法候选词乱码的情况。  
显示是乱码，按space选择出来的词却是对的。  
解决办法一条命令：  
```
$ rm -rf ~/.config/SogouPY ~/.config/sogou*  
```
然后重启电脑就好啦。