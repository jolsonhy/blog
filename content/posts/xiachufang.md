---
title: 前端学习笔记
date: 2016-04-13 22:42:28
tags: [学习笔记]
categories: [学习笔记]
---

## 问题一：外容器的尺寸未知,内容器已知，实现水平垂直居中

### html结构:
```html
<div class="container">
    <div class="item"></div>
</div>
```

### 方法一：绝对定位 + 负外边距
```css
.container {
    position: relative;
}
.item {
    width: 200px;
    height: 200px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin: -100px 0 0 -100px;
}
```

### 方法二：绝对定位 + 四周偏移量定位0， 外边距为auto
```css
.container {
    position: relative;
}
.item {
    width: 200px;
    height: 200px;
    position: absolute;
    margin: auto;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
}
```

### 方法三：Flex定位
```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
.item {
    width: 200px;
    height: 200px;
}
```

### 方法四： CSS3 tramsform属性
```css
.container {
    position: relative;
}
.item {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    height: 200px;
}
```

### 方法五： tabel-cell
```css
.container {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}
.item {
    width: 200px;
    height: 200px;
}
```


## 问题二：实现固定导航栏
### 方法一：fixed固定定位
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>固定导航栏</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        body {
            height: 2000px;
            overflow-y: scroll;
        }
        .nav {
            position: fixed;
            width: 100%;
            height: 200px;
            top: 0;
            background-color: lightskyblue;
        }
    </style>
</head>
<body>
    <div class="nav"></div>
</body>
</html>
```

### 方法二：导航栏绝对定位，js监听滚动事件
```javascript
(function (){
    var nav = document.getElementsByClassName('nav'),
        offTop = document.offsetTop,
        scrTop = document.documentElement.scrollTop || document.body.scrollTop;

    window.addEventListener('scroll', function () {

        if (scrTop > offTop) {
            nav.style.position = 'absolute';
            nav.style.top = scrTop;
            nav.style.margin = '0';
        }
    }, false);
}());
```


## 问题三
在Chrome的控制台运行` var log = console.log; log(123) `，报错信息为：` Illegal invocation `

### 错误原因：
console对象是JavaScript的原生对象,这里在全局范围内定义一个log函数，实际上在浏览器的工作环境下是给window全局变量添加了一个window.log()方法，实际执行的是window.log(123),故报错。
### 一个可成功执行的log函数：
```javascript
var log = function (a) {
	console.log(a);
};

log(123);
```


## 问题四
不使用jQuery让方块B实现与方块A完全相同的效果。
由于没有研究过jQuery的源码，所以对其`.animate()`的实现没有了解，查询资料后得知，` .animate(prop [, speed] [, easing] [, callback]) `接收四个参数，而题目中仅给了一个进行动画的样式，故`duration`参数为默认值`400ms`, `easing`为默认值余弦动画'swing'
swing的动画函数为：
```javascript
swing: function( p, n, firstNum, diff ) {
       return ( ( -Math.cos(p * Math.PI) / 2 ) + 0.5 ) * diff + firstNum;
}
```
尝试后无法实现该余弦函数动画，以下是接近的非余弦的：

```javascript
(function () {
    setInterval(function() {
        var b = document.getElementById('b');
        var speed = (500 - b.offsetLeft) / 15;
        speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);
        b.style.left = b.offsetLeft + speed + 'px';
    }, 13)
}());
```


## 问题五
使用现代浏览器的 Promise 编写一个ajax工具函数。
```javascript
/**
 * 一个ajax工具函数
 * @param {string} url - 请求的url
 * @param {string} method - 请求的method，GET、POST等
 * @return {Promise.<string|Error>}
 */
var ajax = function (url, method) {
    var promise = new Promise(function (resolve, reject) {
        var client = new XMLHttpRequest();
        client.open(method, url);
        client.onreadystatechange = handle;
        client.responseType = 'json';
        client.setRequestHeader('Accept', 'application/json');
        client.send();

        function handle() {
            if (this.readyState !== 4) {
                return;
            }
            if (this.status >= 200 && this.status < 300) {
                resolve(this.response);
            } else {
                reject(new Error(this.statusText));
            }
        }
    });

    return promise;
};
```

## 问题六
最近在读阮一峰老师的《ES6标准入门》一书，因为近期在跟着做百度ife前端学院的春季任务，希望一边学习使用ES6的新语法糖完成，目前用的比较多的有箭头函数、解构赋值、let等。
