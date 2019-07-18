---
title: Linux vim设置tab缩进
date: 2015-10-24 10:59:12
tags: [Linux]
categories: [学习笔记]
---

## 前言

由于本周操作系统实验是使用Linux vim实现短作业优先算法，而在编辑过程中发现vim没有默认缩进以及代码行号等功能，故搜索后设置。

## 正文

1、在自己的家目录下建立.vimrc文件。终端输入vi ~/.vimrc 回车。 

2、在.vimrc文件中输入如下文本

``` bash
set tabstop=4 
set softtabstop=4 
set shiftwidth=4 
set noexpandtab 
set nu  
set autoindent 
set cindent
``` 

### 代码解释：

* Tabstop:表示一个 `tab` 显示出来是多少个空格的长度,默认为8，我设置为4。
* Softtabstop:表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用 `expandtab` 时特别有用。
* Shiftwidth:表示每一级缩进的长度，一般设置成跟 `softtabstop` 一样。 当设置成 `expandtab` 时，缩进用空格来表示`noexpandtab` 则是用制表符表示一个缩进。
* Nu:表示 `显示行号` 。
* Autoindent:表示 `自动缩进` 。
* Cindent:是特别针对 `C语言` 自动缩进。 

3、设置完后保存退出。运行 `source ~/.vimrc` 使配置文件生效。即可体验按tab键时缩进4个空格的宽度，C编程时换行自动缩进。 