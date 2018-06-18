---
title: 开发环境更换为Window
date: 2018-05-08 09:57:31
tags:
---
## 工作需要，开发环境更换为Windows
### 一开始不太习惯  
大概用了半年多的Linux，从一开始用的Ubuntu 16.04到用的最长时间的Linux mint。习惯了在Linux上搭建前端开发环境。各种开发库都在Terminal里面安装。除了个别软件不支持外，用起来还是很舒服很稳定的。微信可以用[electronic-wechat](https://github.com/geeeeeeeeek/electronic-wechat)代替，fq有qt5的ss。没办法的是，Linux对QQ支持不好，而且之后要开发小程序，为了比较稳定的工作环境，所以我还是选择回到Windows。
### 对于前端开发而言，需要的其实不多
* vscode
* node
* git
* Google
其实，当就开发而言，这几个就足够了。
### 转移到Windows
1. 安装必要的开发工具。
* git
* node
* vscode
* 微信小程序开发工具
* TortoiseSVN（还在熟悉中...）
2. 同步vscode设置、插件   
vscode要重新设置？不存在的。  
vscode有一个“setting sync”的插件，使用github 生成的token和gist id就可以同步vscode的基本插件了。
* 上传：在原Linux的vscode上安装setting sync，通过快捷键`Shift+Alt+u`，按提示生成token填入就可以。
* 下载：在Windows的vscode上安装setting sync，`Shift+Alt+d`，输入token和gist id就可以下载了。
### 在习惯中  
用了大概一天时间，发现，还挺好的🤦‍。