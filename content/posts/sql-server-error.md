---
title: 安装SQL Server出现29506错误的解决方案
date: 2015-09-25 23:25:12
tags: [SQL Server, 安装]
categories: [学习笔记]
---

前言
------
此文记录Win8.1(64位)安装SQL Server Management Studio Express时出现[error 29506]错误的解决方案。

## 文章 ##

 1. Microsoft SQL Server Management Studio Express:
 	下载地址：[Microsoft SQL Server Management Studio Express][1]

 2. 解决方案原文地址:
 	[SQL Server Management, error 29506][2]

## 正文 ##

 1. 把`SQLServer2005_SSMSEE_x64.msi`拷贝到`C盘根目录`下（不一定，随后需要DOS方便进入这个目录就行）。
 2. 开始(左下角的Windows徽标) -> 所有程序 -> 附件 -> 命令提示符，选中后右键 -> 以管理员身份运行。
 3. 进入DOS界面后，通过cd命令转到SQLServer2005_SSMSEE_x64.msi的目录下，输入`SQLServer2005_SSMSEE_x64.msi`回车即可安装。

  [1]: http://www.microsoft.com/downloads/details.aspx?displaylang=zh-cn&FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796
  [2]: http://social.msdn.microsoft.com/Forums/en/sqlexpress/thread/3a8b79a2-9258-464f-bdb8-70e77d5f0f1a