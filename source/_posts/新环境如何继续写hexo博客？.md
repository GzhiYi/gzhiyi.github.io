---
title: 新环境如何继续写hexo博客？
date: 2018-05-08 10:35:25
tags:
---
## hexo 默认下上传的是部署到github pages的代码|文件
![默认代码](http://p7b9iw239.bkt.clouddn.com/default_code.png)
### 那怎么继续写博客呢？
1. 在原电脑的博客仓库新建一个源代码分支：
```bash
git checkout -b source-code
git add .
git commit -m "added source-code branch"
git push origin source-code
```
之后代码将同步到github。

<!--more-->

2. 新电脑拉代码，切到source-code分之
```bash
git checkout source-code
yarn                          // 安装packages
hexo server                   // 本地查看博客（跑起来）
```
和之前一样，写博客，然后部署
```bash
hexo g -d
```
**注意**：disqus需要fq才能访问，所以看不到评论的话，你懂的。