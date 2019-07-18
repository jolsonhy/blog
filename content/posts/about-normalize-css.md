---
title: 关于样式重置：normalize.css
date: 2015-08-04 20:56:28
tags: [CSS]
categories: [学习笔记]
---


------
## 前言 
这几天在Udacity上学习Front-End Web Developer Nanodegree的课程，从里面发现了很多很实用又以前未接触的东西，包括今天看到的css resetting，之前一直用的`* {margin:0; padding: 0;}`来简单处理这个问题，看到Udacity的工程师提到Normalize，查询资料后才知道其妙用。



关于reset.css
-----------

关于CSS样式重置问题，一直都存在很多争议，有人觉得可以直接使用前言所说的`* {margin:0; padding: 0;}`，而有人则觉得这种方法会对性能有影响，而提倡使用比较流行的[Eric Meyer][1]和[YUI][2]的重置样式。
但貌似随着CSS的发展，更多人开始觉得此种样式重置没有必要。下面贴两篇同一个作者时隔一年提出两种截然相反的观点的文章：

 - [打造自己的RESET.CSS][3]
 - [真的还需要RESET.CSS么？][4]

关于normalize.css
---------------

 而今天要说的则是目前很多人支持并且广泛使用的normalize.css。
它 只是一个很小的CSS文件，但在默认的HTML元素样式上提供了跨浏览器的高度一致性。相比于传统的CSS reset，Normalize.css是一种现代的、为HTML5准备的优质替代方案。Normalize.css现在已经被用于Twitter Bootstrap、HTML5 Boilerplate、GOV.UK、Rdio、CSS Tricks 以及许许多多其他框架、工具和网站上。

## Normalize vs Reset ##
知道Normalize.css和传统Reset的区别是非常有价值的。

### normalize.css 保护了有价值的默认值

`Reset`通过为几乎所有的元素施加默认样式，强行使得元素有相同的视觉效果。相比之下，Normalize.css保持了许多默认的浏览器样式。这就意味着你不用再为所有公共的排版元素重新设置样式。当一个元素在不同的浏览器中有不同的默认值时，Normalize.css会力求让这些样式保持一致并尽可能与现代标准相符合。

### normalize.css 修复了浏览器的bug

它修复了常见的桌面端和移动端浏览器的bug。这往往超出了Reset所能做到的范畴。关于这一点，Normalize.css修复的问题包含了HTML5元素的显示设置、预格式化文字的font-size问题、在IE9中SVG的溢出、许多出现在各浏览器和操作系统中的与表单相关的bug。

可以看以下这个例子，看看对于HTML5中新出现的input类型search，Normalize.css是如何保证跨浏览器的一致性的。
  
``` css
    /**  
     * 1. Addresses appearance set to searchfield in S5, Chrome  
     * 2. Addresses box-sizing set to border-box in S5, Chrome (include -moz to future-proof)  
     */

    input[type="search"] {   
        -webkit-appearance: textfield; /* 1 */  
        -moz-box-sizing: content-box;   
        -webkit-box-sizing: content-box; /* 2 */   
        box-sizing: content-box; 
    }

    /**  
     * Removes inner padding and search cancel button in S5, Chrome on OS X  
     */

    input[type="search"]::-webkit-search-decoration, 
    input[type="search"]::-webkit-search-cancel-button {  
        -webkit-appearance: none; 
    }
```

### normalize.css 不会让你的调试工具变的杂乱

使用Reset最让人困扰的地方莫过于在浏览器调试工具中大段大段的继承链，如下图所示。在Normalize.css中就不会有这样的问题，因为在我们的准则中对多选择器的使用时非常谨慎的，我们仅会有目的地对目标元素设置样式。

### normalize.css 是模块化的

这个项目已经被拆分为多个相关却又独立的部分，这使得你能够很容易也很清楚地知道哪些元素被设置了特定的值。因此这能让你自己选择性地移除掉某些永远不会用到部分（比如表单的一般化）。

### normalize.css 拥有详细的文档

Normalize.css的代码基于详细而全面的跨浏览器研究与测试。这个文件中拥有详细的代码说明并在Github Wiki中有进一步的说明。这意味着你可以找到每一行代码具体完成了什么工作、为什么要写这句代码、浏览器之间的差异，并且你可以更容易地进行自己的测试。

这个项目的目标是帮助人们了解浏览器默认是如何渲染元素的，同时也让人们很容易地明白如何改进浏览器渲染。
    
  
## 如何使用 normalize.css ##
首先，安装或从Github下载[Normalize.css][5]，接下来有两种主要途径去使用它。

 - 策略一：将normalize.css作为你自己项目的基础CSS，自定义样式值以满足设计师的需求。
 - 策略二：引入normalize.css源码并在此基础上构建，在必要的时候用你自己写的CSS覆盖默认值。
 
## 结语 ##
无论从适用范畴还是实施上，Normalize.css与Reset都有极大的不同。尝试一下这两种方法并看看到底哪种更适合你的开发偏好是非常值得的。这个项目在Github上以开源的形式开发。任何人都能够提交问题报告或者提交补丁。整个项目发展的过程对所有人都是可见的，而每一次改动的原因也都写在commit信息中，这些都是有迹可循的。

## 参考 ##

 - [Normalize.css官网][6]
 - [Normalize.css 在GitHub上的源码][7]



  [1]: http://meyerweb.com/eric/thoughts/2007/04/14/reworked-reset/
  [2]: http://yui.github.io/yui2/
  [3]: http://shawphy.com/2009/03/my-own-reset-css.html
  [4]: http://shawphy.com/2010/09/no-css-reset.html
  [5]: http://necolas.github.io/normalize.css/
  [6]: http://nicolasgallagher.com/about-normalize-.html
  [7]: https://github.com/necolas/normalize.css