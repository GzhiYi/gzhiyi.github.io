---
title: 基于CRA，使用Redux、Router、Sass等快速搭建纯前端React项目
date: 2018-08-20 09:37:03
tags: 
categories: React
---
本项目基于[`Create React App`](https://github.com/facebookincubator/create-react-app)
仓库地址：[https://github.com/GzhiYi/react-onestep](https://github.com/GzhiYi/react-onestep)
### 克隆

```bash
git clone git@github.com:GzhiYi/react-onestep.git
cd react-onestep
yarn
```
可选npm包管理安装package`npm install`  
如果不想要提交历史，可以克隆后手动删除.git文件夹后再初始化仓库。
<!--more-->
### 分支
暂无

### 内建命令

具备Create React App命令，但经过弹出原始隐藏配置后，`npm run eject`或者`yarn run eject`不可用。

#### `npm start` 或 `yarn start`

运行开发模式，在运行前会编译scss为css文件，运行后打开[http://localhost:3000](http://localhost:3000)即可看到项目内容。

#### `npm test` 或 `yarn test`

运行测试，未基于原项目再次补充测试用例等代码。

#### `npm run build` 或 `yarn run build`

运行生产模式打包项目用于部署。生产模式将通过webpack压缩代码，生成的文件名附带哈希值。

### 项目额外包含内容

* 项目预留两个小栗子，便于了解React状态管理和路由的知识。
* 生成vscode调试文件，可以安装扩展`Debugger for Chrome`调试程序。可自行修改`launch.json`以符合你的要求。
* 支持`sass`。运行开发模式将自动编译为对于的css文件。组件内需要手动引入对应生成的css文件。若在新文件夹内新建`scss`文件，务必在`.gitignore`内增加排除，保持线上仓库整洁。
* 使用[normalize.css](https://github.com/necolas/normalize.css)。
* flow静态类型检查，写代码不至于那么多bug。查看[文档](https://flow.org/)了解用法。
* 引入`react-redux`、`react-router-dom`、`redux-thunk`等，便于搭建大型应用。
* 提供`redux-logger`，可在console内看到状态，方便调试控制。
* 使用`axios`请求接口。
* 分离模块，对项目结构进行控制，可以有序的管理组件和state。
* 不间断寻找更优方案优化项目。
* ...

### 主要模块文件夹说明

1. `config` webpack配置文件以及jest配置文件等。如重点关注视图层开发，可不用过多关注相关配置。
2. `script` 命令入口处，可以调整build、start、test命令相关的配置。
3. `src/modules` 分布actions、reducers以及constants.js，用于处理数据请求、状态数据等相关操作。
4. `src/settings` 配置http实例、路由设置以及状态管理的配置文件。
5. `src/views` 视图层、组件分布。

### 其他

如果只是开发一个简单的应用，可以直接用CRA创建一个。而对于redux这样的状态管理，如果应用状态少，大可不需要使用，你完全可以使用React本身的state处理组件的状态。这个项目适合构建较为复杂的应用，适合多人分工构建。  

再有，当前master分支只有搭建好的项目结构，可以随时通过npm引入其他需要的package。由于项目刚搭建不久，可以用于个人网站或者稍微复杂的页面，后续不断提升项目稳定性和易用性，争取能开箱即用，快速搭建需要的页面内容。

有更好的实践或者方案，欢迎留言讨论PR。
