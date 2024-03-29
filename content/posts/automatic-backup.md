---
title: 自动备份Hexo博客源文件
date: 2015-12-06 08:38:24
tags: [hexo]
categories: [学习笔记]
---

## 前言
这两天给电脑换上了SSD和内存，系统重装，一堆的软件和配置真是折腾死了。得益于之前加入的一个博文自动备份到Github上的脚本，让我没有再为这个费头脑。所以此文记录自动备份Hexo博客源文件到Github的过程。

原文出自：[自动备份Hexo博客源文件](http://notes.xiamo.tk/2015-07-06-%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BDHexo%E5%8D%9A%E5%AE%A2%E6%BA%90%E6%96%87%E4%BB%B6.html)

## 原理
通过通过监听Hexo的其它事件来完成自动执行Git命令完成自动备份。
通过查阅Hexo文档，找到了Hexo的主要事件，见下表：

事件名 　|	事件发生时间
----|------
deployBefore  |  在部署完成前发布
deployAfter	 |  在部署成功后发布
exit  |  在 Hexo 结束前发布
generateBefore  |  在静态文件生成前发布
generateAfter  |  在静态文件生成后发布
new  |  在文章文件建立后发布

于是我们就可以通过监听Hexo的deployAfter事件，待上传完成之后自动运行Git备份命令，从而达到自动备份的目的。


## 实现

### 将Hexo目录加入Git仓库
1、在Github下创建一个新的repository，取名为HEXO。(与本地的Hexo源码文件夹同名即可)进入本地的Hexo文件夹，执行以下命令创建仓库:
 ```bash
 git init
 ```
 	

2、设置远程仓库地址，并更新:
 ```bash
 git remote add origin git@github.com:XXX/XXX.git
 git pull origin master
 ```
 	

3、修改`.gitignore`文件（如果没有请手动创建一个），在里面加入`*.log` 和 `public/` 以及`.deploy*/`。因为每次执行`hexo generate`命令时，上述目录都会被重写更新。因此忽略这两个目录下的文件更新，加快push速度。

4、执行命令以下命令，完成Hexo源码在本地的提交:
 ```bash
 git add .
 git commit -m "添加hexo源码文件作为备份"
 ```
 	

5、执行以下命令，将本地的仓库文件推送到Github:
 ```bash
 git push origin master
 ```
 	

### 安装shelljs模块
要实现这个自动备份功能，需要依赖`Node.js`的一个`shelljs`模块,该模块重新包装了child_process,调用系统命令更加的方便。该模块需要安装后使用。

在命令中键入以下命令，完成shelljs模块的安装：
```bash
npm install --save shelljs
```

### 编写自动备份脚本
待到模块安装完成，在Hexo根目录的scripts文件夹下新建一个js文件，文件名随意取。

ps: 如果没有scripts目录，请新建一个。

然后在脚本中，写入以下内容：
``` javascript
require('shelljs/global');

try {
	hexo.on('deployAfter', function() {//当deploy完成后执行备份
		run();
	});
} catch (e) {
	console.log("产生了一个错误<(￣3￣)> !，错误详情为：" + e.toString());
}

function run() {
	if (!which('git')) {
		echo('Sorry, this script requires git');
		exit(1);
	} else {
		echo("======================Auto Backup Begin===========================");
		cd('D:/hexo');    //此处修改为Hexo根目录路径
		if (exec('git add --all').code !== 0) {
			echo('Error: Git add failed');
			exit(1);

		}
		if (exec('git commit -am "Form auto backup script\'s commit"').code !== 0) {
			echo('Error: Git commit failed');
			exit(1);

		}
		if (exec('git push origin master').code !== 0) {
			echo('Error: Git push failed');
			exit(1);

		}
		echo("==================Auto Backup Complete============================")
	}
}
```

其中，需要修改第17行的D:/hexo路径为Hexo的根目录路径。（脚本中的路径为博主的Hexo路径）

如果你的Git远程仓库名称不为origin的话，还需要修改第28行执行的push命令，修改成自己的远程仓库名和相应的分支名。

保存脚本并退出，然后执行hexo deploy命令，将会得到类似以下结果:

```bash
INFO  Deploying: git>
INFO  Clearing .deploy folder...
INFO  Copying files from public folder...
[master 3020788] Site updated: 2015-07-06 15:08:06
 5 files changed, 160 insertions(+), 58 deletions(-)
Branch master set up to track remote branch gh-pages from git@github.com:smilexi
amo/notes.git.
To git@github.com:smilexiamo/notes.git
   02adbe4..3020788  master -> gh-pages
On branch master
nothing to commit, working directory clean
Branch master set up to track remote branch gitcafe-pages from git@gitcafe.com:s
milexiamo/smilexiamo.git.
To git@gitcafe.com:smilexiamo/smilexiamo.git
   02adbe4..3020788  master -> gitcafe-pages
INFO  Deploy done: git
======================Auto Backup Begin===========================
[master f044360] Form auto backup script's commit
 2 files changed, 35 insertions(+), 2 deletions(-)
 rewrite db.json (100%)
To git@github.com:smilexiamo/hexo.git
   8f2b4b4..f044360  master -> master
==================Auto Backup Complete============================
```

这样子，每次更新博文并deploy到服务器上之后，备份就自动启动并完成备份啦

