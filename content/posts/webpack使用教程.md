---
title: Webpack使用
date: 2016-04-09 11:55:55
tags: [教程, Webpack]
categories: [学习笔记]
---


## 前言

最近在做百度前端技术学院的任务时，尝试接触使用了ES6新的语法糖，在这过程中，也顺便学习了一下近期很火的webpac的使用，以下是对近期学习的一个总结与归纳。

## 概要

`Webpack`不同于`Gulp`或者`Grunt`，后者是一种自动化处理工具，而`Webpack`是一个种模块化解决方案，名为`MODULE BUNDLER`。

在 webpack 里，所有类型的文件都可以是模块，包含最常见的 JavaScript，以及 css 文件、图片、json 文件等等。通过 webpack 的各种加载器，可以更有效地管理这些文件。

## 安装

- 全局安装
```bash
npm install webpack -g
```

- 本地安装
```bash
npm init
npm install webpack --save-dev
```

- 查看所有命令
```bash
webpack --help
```

## Webpack配置

Webpack默认的配置文件名称为`webpack.config.js`，其中包含了一个`entry`入口和一个`output`出口
```javascript
// webpack.config.js
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  }
};
```

在创建好`webpack.config.js`文件后，直接使用`webpack`命令即可，下面是命令的几种模式：

```bash
webpack // 构建为开发环境输出
webpack -p // 构建为生产环境输出（压缩混淆脚本）
webpack --watch // 监听变动并自动打包
webpack -d // 生成map映射文件，告知哪些模块被最终打包到哪里了
```

## 模块化

### 模块化JavaScript
1. 安装babel-loader
```bash
npm install babel-loader babel-core babel-preset-es2015 --save-dev
```

2. 配置 `webpack.config.js`
在 `module.exports` 值中添加 module：

```javascript
module.exports = {
entry: {
    app: ['./main.js']
},
output: {
    filename: 'bundle.js'
},
module: {
    loaders: [{
        test: /\.js$/,
        loaders: ['babel?presets[]=es2015'],
        exclude: /node_modules/
    }]
}
}
```

### CSS加载器
我们可以按传统方法使用 CSS，即在 HTML 文件中添加：

```css
<link rel="stylesheet" href="style/app.css">
```

但 webpack 里，CSS 同样可以模块化，使用 import 导入。

因此我们不再使用 link 标签来引用 CSS，而是通过 webpack 的 [style-loader](https://github.com/webpack/style-loader) 及 [css-loader](https://github.com/webpack/css-loader)。前者将 css 文件以 `<style></style>` 标签插入 `<head>` 头部，后者负责解读、加载 CSS 文件。

#### 安装 CSS 相关的加载器
```bash
npm install style-loader css-loader --save-dev
```

#### 配置 webpack.config.js 文件
```javascript
{
// ...
module: {
    loaders: [
        { test: /\.css$/, loaders: ['style', 'css'] }
    ]
}
}
```
#### 在 main.js 文件中引入 css
```javascript
import'./style/app.css';
```
这样，在执行 `webpack` 后，我们的 CSS 文件就会被打包进 `bundle.js` 文件中，如果不想它们被打包进去，可以使用 [extract text](https://github.com/webpack/extract-text-webpack-plugin)扩展。


### autoprefixer

我们在写 CSS 时，按 CSS 规范写，构建时利用 autoprefixer 可以输出 -webkit、-moz 这样的浏览器前缀，webpack 同样是通过 loader 提供该功能。

#### 安装 autoprefixer-loader
```bash
npm install autoprefixer-loader --save-dev
```

#### 配置 webpack.config.js
```bash
loaders: [{
test: /\.css$/,
loader: 'style!css!autoprefixer?{browsers:["last 2 version", "> 1%"]}',
//...
}]
```


## 完整示例
```javascript
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');

module.exports = {
    //插件项
    plugins: [commonsPlugin],
    //页面入口文件配置
    entry: {
        index : './src/js/page/index.js'
    },
    //入口文件输出配置
    output: {
        path: 'dist/js/page',
        filename: '[name].js'
    },
    module: {
        //加载器配置
        loaders: [
            //.css 文件使用 style-loader 和 css-loader 来处理
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            //.js 文件使用 jsx-loader 来编译处理
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
    }
    //其它解决方案配置
    resolve: {
        root: 'E:/github/flux-example/src', //绝对路径
        extensions: ['', '.js', '.json', '.scss'],
        alias: {
            AppStore : 'js/stores/AppStores.js',
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
};
```
