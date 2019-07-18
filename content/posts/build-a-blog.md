---
title: 搭建Hexo博客过程
date: 2015-10-04 21:07:12
tags: [hexo, 教程]
categories: [学习笔记]
---

## 搭建前的准备

 1. 安装Hexo前，电脑上需要有下面两个应用程序：
  - [Node.js](http://nodejs.org/)
  - [Git](http://git-scm.com/)

 Git和Node.js的安装过程在此不详细展开，可参照官网的教程。


## 搭建过程

### 使用npm安装Hexo

``` bash
$ npm install -g hexo-cli
````

ps：此过程如果没有翻墙可能会出现一直在下载安装但很久都没有有提示成功的情况，这时候可以换一种方式：
采用淘宝街镜像源安装：

``` bash
npm install -g hexo-cli --registry=https://registry.npm.taobao.org
```

### 建站

``` bash
mkdir <folder>
cd hexo
hexo init
npm install
```

新建完成后，制定文件夹的目录如下：

``` bash
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

（ps：此目录来自官网：[Hexo建站](https://hexo.io/zh-cn/docs/setup.html)）

### 3. 本地生成和查看

``` bash
hexo server
```

然后直接登录 [http://localhost:4000](http://127.0.0.1:4000) 就可以看到本地的默认风格的 Hexo Blog 了。


## 生成本地SSH Key

``` bash
$ssh-keygen -t rsa -C "youremail@example.com"  //创建SSH Key
$git config --global user.name "yourname"  
$git config --global user.email "youremail@example.com"  
$cd ~/.ssh/  
$cat id_rsa.pub  //这里的id_rsa.pub是公钥，可以告诉任何人
```

> 这里的SSH key之后绑定`Github`和`Gitcafe`都需要用到。


## 生成Github Pages

1、给GitHub配置生成的SSH Key，路径是GitHub右上角的小齿轮(设置入口) -> SSH keys -> Add SSH Keys -> 输入title -> 把id_rsa.pub里的内容复制到Key里 -> 提交。

![添加SSH Key到Github里](http://7xkus6.com1.z0.glb.clouddn.com/build-a-blog-1.png)

2、新建一个Repository，假如你的Github名字为michael，这个仓库名就是：michael.github.io，即：

``` bash
用户名.github.io
```

> 具体操作可直接参考[Github Pages官方教程](https://pages.github.com/)。


## 生成Gitcafe Pages

Gitcafe Pages和Github的类似，但需要注意的是，其仓库名应该是与用户名完全一样，然后自己的代码都放在gitcafe-pages分支上，这一点与Github不同，其他操作类似。

> 具体操作可直接参考[Gitcafe Pages官方教程](https://gitcafe.com/GitCafe/Help/wiki/Pages-%E7%9B%B8%E5%85%B3%E5%B8%AE%E5%8A%A9?locale=zh-CN)。


## Hexo全局配置

用文本编辑器修改_config.yml这个文件，大致如下，只需要自行修改几个，其他保持默认即可。

通常需要修改站点名称、URL格式、归档设置、disqus评论用户名、部署配置这几项就可以了，注意冒号后面都要添加一个半角空格，之后才是设置参数。

自定义域名设置：

在source文件夹下面新建CNAME文件，里面写入你的自定义域名，并设置您的DNS配置CNAME方式到服务提供商的给的地址即可。

``` bash
//网站
参数  //描述
title   //网站标题
subtitle    //网站副标题
description //网站描述
author  //您的名字
language    //网站使用的语言
timezone    //网站时区。Hexo 预设使用您电脑的时区。时区列表

//网址
//参数、描述、默认值
url  //网址  
root  //网站根目录   
permalink  //文章的永久链接,格式 :year/:month/:day/:title/
permalink_default  //永久链接中各部分的默认值    
网站存放在子目录
如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。

// 目录
//参数  描述  默认值
source_dir  //资源文件夹，这个文件夹用来存放内容。  source
public_dir  //公共文件夹，这个文件夹用于存放生成的站点文件。 public
tag_dir 标签文件夹  //tags
archive_dir 归档文件夹  // archives
category_dir    分类文件夹  // categories
code_dir    Include code 文件夹   // downloads/code
i18n_dir    国际化（i18n）文件夹   // :lang
skip_render 跳过指定文件的渲染，您可使用 glob 来配置路径。  

// 文章
//参数  描述  默认值
new_post_name   //新文章的文件名称   :title.md
default_layout  //预设布局   post
auto_spacing    //在中文和英文之间加入空格    false
titlecase  // 把标题转换为 title case   false
external_link   //在新标签中打开链接   true
filename_case  // 把文件名称转换为 (1) 小写或 (2) 大写 0
render_drafts   //显示草稿    false
post_asset_folder   //启动 Asset 文件夹    false
relative_link   //把链接改为与根目录的相对位址  false
future  //显示未来的文章 true
highlight  // 代码块的设置  

// 分类 & 标签
//参数  描述  默认值
default_category  //  默认分类    uncategorized
category_map   // 分类别名    
tag_map //标签别名    
日期 / 时间格式
Hexo 使用 Moment.js 来解析和显示时间。

//参数  描述  默认值
date_format //日期格式    MMM D YYYY
time_format //时间格式    H:mm:ss
//分页
//参数  描述  默认值
per_page   // 每页显示的文章量 (0 = 关闭分页功能)   10
pagination_dir  //分页目录    page

// 扩展
//参数  描述
theme  // 当前主题名称
deploy  //部署
```


## Hexo部署

### 修改 hexo 根目录下 _config.yml 文件(‘xxxx’为你的用户名)：

 ``` bash
 deploy:
   type: git
   message: Site updated
   repo:
     github: https://github.com/xxxx/xxxx.github.io.git,master
     gitcafe: https://gitcafe.com/xxxx/xxxx.git,gitcafe-pages
 ```

### 安装 hexo-deployer-git

 ``` bash
 npm install hexo-deployer-git --save
 ```

### 更新hexo到最新版

 ``` bash
 npm update hexo -g
 ```

### 写作命令

 - 建立新文章：`hexo n "新文章名"`
 - 预览文章：`hexo s`
 - 生成网页：`hexo g`
 - 发布文章：`hexo d`
 - 生成网页并发布文章：`hexo d -g`


## NameServer和DNSPod

 1. 去域名服务商页面把 NameServer 设为 DNSPod 的 NS：f1g1ns1.dnspod.net 和 f1g1ns2.dnspod.net.

 2. DNSPod 的设定见图(红色的部分是 GitHub 的账户名称)：

 ![pic1](http://7xkus6.com1.z0.glb.clouddn.com/build-a-blog-2.png)
 ![pic2](http://7xkus6.com1.z0.glb.clouddn.com/build-a-blog-3.png)
