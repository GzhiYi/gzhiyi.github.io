---
title: 聊一下reveal.js
date: 2018-07-30 22:38:27
tags: JavaScript
---
### 介绍
The HTML Presentation Framework `https://revealjs.com`  
最近由于有个演讲，需要准备ppt。满心的准备打开我office，仔细想了下，我之前不是见过一个开源的做ppt的js框架嘛。。赶紧上github瞧瞧，好家伙，都已经40K stars了。所以，可以趁机看看问问写写哈哈。  
<!--more-->
### 使用
在你将关键的文件引入一个新建的html后，就可以写下第一页网页ppt了。
```html
<html>
  <head>
    <link rel="stylesheet" href="css/reveal.css">
    <link rel="stylesheet" href="css/theme/white.css">
  </head>
  <body>
  <div class="reveal">
    <div class="slides">
      <section>Slide 1</section>
        <section>Slide 2</section>
      </div>
    </div>
    <script src="js/reveal.js"></script>
    <script>
      Reveal.initialize();
    </script>
  </body>
</html>
```
实际上，使用起来比想象中还要容易，外层是一个`.reveal`，内部一个`.slides`。最后在section写一个页面的布局等就ok了。需要我多说用法？不了不了，这很容易，github或者主页有详尽的api。很好理解，我差的不是不会写html，不会写css，我只是差不会布局！可以在[这](https://github.com/hakimel/reveal.js)找到你需要的api。

### 结果
你觉得我会写出一个ppt来吗？哈哈，没有，在我做了几页之后就放弃了。
1. 写起来费劲，适合突然之间用纯文字加少量动效撼人的ppt。
2. 使用起来真的没有ppt方便啊。
3. 我要把项目传讲台上播放，万一兼容性呢，我还不知道这个库兼容行如何啊。
4. 算了算了，默默打开Power Point。
或者，有没有人做一个利用这个库的图形化编辑系统呢，毕竟这个播放器来能做能有还能上能下，多好啊！  
还真有。
![微信截图_20180730225104](http://p7b9iw239.bkt.clouddn.com/微信截图_20180730225104.png)
好的，那我直接用这个站点做个出来不是美滋滋
![dog](http://5b0988e595225.cdn.sohucs.com/images/20170830/0d52425e263b43b6bc828284800a447c.jpeg)  
我注册了账号，尝试了一下导出，最后还是放弃了。
1. 免费用户所编辑的slide都是面向大众，ppt内有隐私内容就不行啦。
2. 导出后，不是所有的资源都存在本地，有的样式需要联网，可是，你懂的。
3. 放弃啦，默默打开了我的Power Point，不管了，不回头，赶紧制作！

### 最后
实际上，[slides](https://slides.com/)是真的炒鸡棒，只可惜需要墙，如果在国内我会支持一波的。就不至于会被某些厂家做出像素级模仿的产品出来。最后，ppt昨晚了，讲也讲完了，最近有项目要上线，忙的很，有几篇东西想边学习边写的，时间有点少，后面慢慢计划补上。