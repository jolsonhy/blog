---
title: "Hello, Hugo"
date: 2019-07-17T10:47:06+08:00
# draft: true
tags: [技术, blog]
categories: [Tutorial]
---


## 前言

记得是 16 年还在学校的时候开始折腾博客这个东西，而当时就觉得市面上所有现成的博客系统都做得不大好，要么太丑，要么太重。所以从一开始就走上了自建这么一条不归路，从买服务器，买域名，到接触不同的 Linux 系统，折腾过程中踩了很多坑，但也因此学会了很多东西。如今工作之后，由于没有继续干开发的工作，也很少花时间在这上面了，而以前写的一些东西也因此丢了，心里还是有点难过的。前两天突然心血来潮，觉着这种方式相比朋友圈、微博 Twitter 等东西，更能将过去的一些经验、日常沉淀下来，日后来看，说不定别有另一番滋味，于是，开搞。

记得之前最后一个版本的博客是基于 Hexo 搞的，整体来讲还是不错的，不过记忆中那一堆 Nodejs 的依赖，各种乱七八糟的小问题，折腾了好久，再加上确实当内容多起来之后，每次编译生成的速度会比较慢，所以想着换一个试试，网上搜了一大圈，不少人都推荐 Hugo，于是做了下尝试，确实不错，暂时就用它了。以下是重新用它搭建的整体流程记录。

## 本地安装
### 整个流程如下：

`安装 Hugo` -> `新建站点` -> `选择并拷贝主题到 themes 目录` -> `修改站点配置文件` -> `本地测试运行` -> `完成`

### 整个站点各个目录的作用：
* archetypes：给不同的类型定义默认FrontMatter，
* content：源文件，相当于 hexo 的 source 目录
* data：数据文件，一般用不上
* layouts：模板
* static：静态资源，也就是不需要Hugo处理的静态资源，比如图片等
* themes：第三方主题，将第三方主题拷贝到这个文件夹下即可使用

### 参考资料：
* [Hugo Documentation](https://gohugo.io/documentation/)
* [Hugo LeaveIt](https://github.com/liuzc/LeaveIt)

## 部署上线

这里还是选择了部署在 Github Pages 上，因为方便且能很好地管理源文件。

本地测试确认没问题后，直接在站点目录下输入命令 `hugo` 即可编译，然后将所有内容 push 到 GitHub 上即可。

由于 Hugo 提供了另一种方式来管理源代码与站点文件，即在配置文件 `config.yoml` 中加一条 `publishDir = "docs"` 的选项，便可把所有编译生成的站点文件放在docs文件夹里，而 GitHub 能在设置中手动将 Pages 的目录选为 docs，这样便能达成一次 push，同时保存源代码与站点文件的结果，我很喜欢。

![Select Source](https://i.imgur.com/ThImuiY.jpg)

稍等片刻，便能输入 `yourname.github.io` 看到你的博客了。

### 自定义域名

当然还是和以前一样自己弄了个域名，并且发现现在不需要手动增加一个 CNAME 文件，可以直接在设置里填写域名便会自动生成，很方便，好评。

![Custom domain](https://i.imgur.com/KAUTgAH.jpg)

接着只需要在域名服务商那里设置一下 DNS 解析，由于我这次没有继续选择 DNSPOD，而是直接用的 NameSilo 后台的解析，所以改用 A 解析，四个 ip 地址都是 GitHub 提供的，如下：

![DNS Records](https://i.imgur.com/pyPMXvG.jpg)


至此，基本就全部完成，其它都是一些小东西的改动，日后慢慢来。