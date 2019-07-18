---
title: Chrome下Flash崩溃的解决方案
date: 2015-10-04 16:51:28
tags: [解决方案, Chrome]
categories: [学习笔记]
---

## 前言

最近使用Chrome看视频的时候，时常出现Flash崩溃的情况，Google一下解决后，现将解决方案记录下来。

## 正文

1. 地址栏输入`chrome://plugins/`进入插件管理页。
2. 打开详细信息。
	![打开详细信息](http://7xkus6.com1.z0.glb.clouddn.com/chrome-flash-collapse-1.png)

3. 把两个Shockwave Flash中类型为`PPAPI`(独立程序)的那个停用。
	![停用其中一个flash](http://7xkus6.com1.z0.glb.clouddn.com/chrome-flash-collapse-2.jpg)


PS. 有的人是关掉chrome自带的flash(PPAPI)，开启系统自带的flash(NPAPI)才比较稳定，请根据具体情况设置。如果没有NPAPI，可以前往[Adobe Flash Player](http://get.adobe.com/cn/flashplayer)下载。

如果还不能解决：

1. 尝试更新显卡驱动。
2. 在设置界面的最后将“使用硬件加速模式（如果可用）”取消选中。