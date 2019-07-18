---
title: Html5之Canvas
date: 2015-08-26 18:34:25
tags: [Html5]
categories: [学习笔记]
---


------

### 绘制2D图形：
* `var canvas = document.getElementById('canvas');`
* `var context = canvas.getContext('2d');`

### 一条路径开始：
* `context.beginPath();  //状态开始`

### 结束：
* `context.closePath();  //状态结束`

### 线段起始位置：
* `context.moveTo(100, 100); //起点坐标`
* `context.lineTo(400, 400); //终点坐标` 

### 如果有多条相连的线段，可用多个lineTo直接首尾相连：
* `context.moveTo(100, 100);`
* `context.lineTo(400, 400);`
* `context.lineTo(100, 400);`
* `context.lineTo(100, 100);` 

### 设置线条属性：
* `context.lineWidth = 5; //线条宽度`
* `context.strokeStyle = 'red'; //线条颜色`

### 对于一个封闭图形，可以填充颜色：
* `context.fillStyle = 'rbg(2, 100, 30)'; //填充颜色`
*  `context.fill();`

### 绘制图形：
* `context.stroke();`