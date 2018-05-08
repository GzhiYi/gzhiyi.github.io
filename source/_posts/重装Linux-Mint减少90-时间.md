---
title: 重装Linux Mint减少90%时间
date: 2018-05-05 22:34:33
categories: "Linux"
---

## 每次重装Linux Mint减少90%时间
### 系统
* [安装Chrome](https://www.google.com/chrome/browser/desktop/index.html)  
    Chrome页面缩放设置为125%，在1080p下看起来舒服些
* [搜狗输入法](https://pinyin.sogou.com/linux/?r=pinyin)  
    重启系统-输入法设置成功-登录QQ账号同步词库
* 安装扩展透明状态栏  
    设置-面板-扩展
* terminal-修改复制粘贴快捷键习惯
    ctrl + shift + c --> ctrl + c  
* 创建ssh key  
[粘贴到此处](https://github.com/settings/keys)
```
sh-keygen
```
* 安装oh my zsh
```
https://github.com/robbyrussell/oh-my-zsh
sudo apt-get install zsh
sudo apt-get install git
```
* 设置小飞机  
    [issue记录](https://github.com/GzhiYi/frontend-log/issues/2)  
* 安装vscode   
    [地址](https://code.visualstudio.com/)  
    下载安装扩展setting-sync 需要token 和 gistId
* 字体问题-linux mint自带字体太丑了 
    软件管理器 - 删除 Fonts-arphic-ukai  Fonts-arphic-uming

### 开发环境搭建
* 安装nvm（node version manager）
```
sudo apt-get update

sudo apt-get install build-essential libssl-dev

curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh

bash install_nvm.sh

nvm use 6.11.5  # 版本
```
* 换TB源  
npm 换源 
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
之后可以用cnpm 代替npm 安装package

yarn 换源 
```
yarn config get registry               //查看yarn源
yarn config set registry 'https://registry.npm.taobao.org'
```
*  安装postgres
```
sudo apt-get install postgresql-client

sudo apt-get install postgresql

sudo apt-get install pgadmin3

sudo su - postgres

psql

create role gzhiyi login;
```
*  安装virtualenv  
```
sudo apt-get install virtualenv
```