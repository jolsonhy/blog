---
title: 前端构建工具Gulp
date: 2016-03-24 22:32:00
tags: [工具使用, 教程, Gulp]
categories: [学习笔记]
---

## 第一步：安装Node
首先需要搭建node环境。访问[Node.js官网](http://nodejs.org)，然后点击大大的绿色的install按钮，下载完成后直接运行程序，就一切准备就绪。npm会随着安装包一起安装，稍后会用到它。

## 第二步：使用命令行
查看node版本号
``` bash
node -v
```
查看npm版本
``` bash
npm -v
```

## 第三步：安装gulp
NPM是基于命令行的node包管理工具，它可以将node的程序模块安装到项目中，在它的官网中可以查看和搜索所有可用的程序模块。
在命令行中输入
``` bash
sudo npm install -g gulp
```

- sudo是以管理员身份执行命令，一般会要求输入电脑密码
- npm是安装node模块的工具，执行install命令
- -g表示在全局环境安装，以便任何项目都能使用它
- 最后，gulp是将要安装的node模块的名字

运行时注意查看命令行有没有错误信息，安装完成后，你可以使用下面的命令查看gulp的版本号以确保gulp已经被正确安装。
``` bash
gulp -v
```
接下来，我们需要将gulp安装到项目本地
``` bash
npm install —-save-dev gulp
```
这里，我们使用*--save-dev*来更新package.json文件，更新*devDependencies*值，以表明项目需要依赖gulp。

Dependencies可以向其他参与项目的人指明项目在开发环境和生产环境中的node模块依懒关系，想要更加深入的了解它可以看看[package.json文档](https://npmjs.org/doc/json.html#dependencies)。


## 第四步：新建Gulpfile文件，运行gulp

安装好gulp后我们需要告诉它要为我们执行哪些任务，首先，我们自己需要弄清楚项目需要哪些任务。

- 检查Javascript
- 编译Sass（或Less之类的）文件
- 合并Javascript
- 压缩并重命名合并后的Javascript

### 安装依赖
``` bash
npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename --save-dev
```

> 提醒下，如果以上命令提示权限错误，需要添加sudo再次尝试。

### 新建gulpfile文件
现在，组件都安装完毕，我们需要新建gulpfile文件以指定gulp需要为我们完成什么任务。

gulp只有五个方法： *task*，*run*，*watch*，*src*，和*dest*，在项目根目录新建一个js文件并命名为*gulpfile.js*，把下面的代码粘贴进去：

#### gulpfile.js
```javascript
// 引入 gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// 检查脚本
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

// 合并，压缩文件
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});

// 默认任务
gulp.task('default', function(){
    gulp.run('lint', 'sass', 'scripts');

    // 监听文件变化
    gulp.watch('./js/*.js', function(){
        gulp.run('lint', 'sass', 'scripts');
    });
});
```

现在，分段解释下这段代码。

### 引入组件
```javascript
var gulp = require('gulp');
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
```
这一步，我们引入了核心的gulp和其他依赖组件，接下来，分开创建lint, sass, scripts 和 default这四个不同的任务。

### Lint任务
```javascript
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});
```
Link任务会检查js/目录下得js文件有没有报错或警告。

### Sass任务
```javascript
gulp.task('sass', function() {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});
```
Sass任务会编译scss/目录下的scss文件，并把编译完成的css文件保存到/css目录中。

### Scripts 任务
```javascript
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});
```
scripts任务会合并js/目录下得所有得js文件并输出到dist/目录，然后gulp会重命名、压缩合并的文件，也输出到dist/目录。

### default任务
```javascript
gulp.task('default', function(){
    gulp.run('lint', 'sass', 'scripts');
    gulp.watch('./js/*.js', function(){
        gulp.run('lint', 'sass', 'scripts');
    });
});
```

这时，我们创建了一个基于其他任务的default任务。使用.run()方法关联和运行我们上面定义的任务，使用.watch()方法去监听指定目录的文件变化，当有文件变化时，会运行回调定义的其他任务。

现在，回到命令行，可以直接运行gulp任务了。
``` bash
gulp
```

这将执行定义的default任务，换言之，这和以下的命令式同一个意思
``` bash
gulp default
```
当然，我们可以运行在*gulpfile.js*中定义的任意任务，比如，现在运行sass任务：
``` bash
gulp sass
```
