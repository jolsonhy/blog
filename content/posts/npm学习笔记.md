---
title: npm学习笔记
date: 2016-05-16 08:59:00
tags: [npm, 学习笔记]
categories: [学习笔记]
---

## 前言
近期在做一个投票项目的应用时，突然发现对npm的认识很少，故查询了一些资料，现将自己的笔记总结在此。

## 介绍
npm是Node.js的包管理工具

## 命令
- 查看用户手册
```bash
npm -h
```

- 生成package.json
```bash
npm init
```

## --save-dev与--save的区别

* `npm install module-name --save-dev` // 把模块和版本号添加到dependencies部分
* `npm install module-name --save` // 把模块和版本号添加到devDependencies部分

devDependencies下列出的模块，是我们开发时用的，比如grunt-contrib-uglify，我们用它混淆js文件，它们不会被部署到生产环境。dependencies下的模块，则是我们生产环境中需要的依赖。