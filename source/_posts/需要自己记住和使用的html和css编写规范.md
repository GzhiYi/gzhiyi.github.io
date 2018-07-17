---
title: 需要自己记住和使用的html和css编写规范
date: 2018-07-17 17:01:04
tags: 
---
### 需要规范
代码写多了，往往在html增加一个属性或者在css上增加一个样式的时候就会不顾及规范，甚至觉得写完就完事。大概在一年前我也翻看了几次mdo的编码规范，我觉得可以遵循，至少能带来一致的呈现。所以决定手动记录一下，好让自己能记得更深，用到真实的项目中去。内容不是100%和原文一致，我只列出自己需要的常用的代码规范。
<!--more-->
### html
- 用两个空格代替tab，已成功转型，项目都是以两个空格缩进的。
- 属性都用双引号，杜绝用单引号。
- 自闭合标签不需要斜杠，比如说img标签。
- 属性的书写顺序：
1. `class`
2. `id, name`
3. `src, for, type, href, value`
4. `title, alt`
- 布尔属性可以不声明值，这个属性有值就是ture，没有就false。

### css
- 用两个空格代替tab。
- 逗号分隔的属性值，在逗号后加一个空格，如： `box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;`。
- 颜色的属性值逗号后不需要加一个空格，如： `background-color: rgba(0,0,0,.5);`。
- 小于1的值省略0，如：`0.5px`要写成`.5px`。
- 十六进制的值要用小写，并使用简写，如6个f写成3个f就好了。如颜色：`#fff`。
- 属性值为0的话`不要`添加单位。如：`margin-left: 0;`。
- 属性声明顺序：
1. position。
2. 盒子模型属性（Box model）。
3. 文本属性（Typographic）。
4. 整体视觉上的（Visual）。
```css
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;
}
```
- 媒体查询到位置：不要打包到一个单一文件或者放到文件的底部上，放在相同规则的附近。
```css
.element { ... }
.element-avatar { ... }
.element-selected { ... }

@media (min-width: 480px) {
  .element { ...}
  .element-avatar { ... }
  .element-selected { ... }
}
```
- 只有一个属性的样式写成一行，方便定位。如：`.span1 { width: 60px; }`。
- 不要滥用简写属性和值。如下很明显：
```css
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```
- 注释：对于较长的注释，务必书写完整的句子；对于一般性注解，可以书写简洁的短语。

[原文](https://codeguide.bootcss.com/)