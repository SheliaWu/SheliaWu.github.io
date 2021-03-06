﻿---
title: Bootstrap的核心特性
date: 2017-02-10 21:36:21
tags: Bootstrap 响应式开发
---

# Bootstrap简介

Bootstrap是一个前端开发框架，来自Twitter。它是基于HTML，CSS和JS编写的，使页面更加简洁灵活，它所提供的样式和组件在构建页面时能省很多功夫。

Bootstrap的源代码大概看了一下还是可以理解的。布局组件是通过给样式给一个类名，在特定的类写特定的CSS代码。JS插件其实也是我们经常练习的功能封装的。

Bootstrap的组件和插件都挺多的，可以大概熟悉个遍，真正使用时再查看相关文档，同时如果想定制自己想要的风格可以在Bootstrap的源代码中更改相关参数。相关文章建议在less和sass文件夹中进行。

Bootstrap最让人惊喜的是它的网格系统，我认为这是它的核心特性。

# 核心特性-网格系统

Bootstrap创建了一个行类名为`.row`，行内规定只能有12列（根据设备大小和你想要的列宽,类名为`.col-xs/sm/md/lg-*`)。

网格系统通过一系列包含内容的行和列来创建页面布局。下面列出了 Bootstrap 网格系统是如何工作的：

-  行必须放置在`.container`，以便获得适当的对齐（alignment）和内边距（padding）。
- 使用行来创建列的水平组，类名为`.row`。
- 内容应该放置在列内，且唯有列可以是行的直接子元素。
- 网格系统是通过指定您想要横跨的十二个可用的列来创建的。例如，要创建三个相等的列，则使用三个` .col-xs-4`。

基本的网格系统代码应该是如下的：

    <div class="container">
       <div class="row">
          <div class="col-*-*"></div>
          <div class="col-*-*"></div>      
       </div>
       <div class="row">...</div>
    </div>

## 网格系统的几个有用的属性

### 列偏移

如，使用` .col-md-offset-*` 类。这些类会把一个列的左外边距（margin）增加` * `列，其中` *` 范围是从 1 到 11。注意偏移是向右的。

### 列排序

如，使用 `.col-md-push-* `和 `.col-md-pull-*` 类的内置网格列的顺序，其中 * 范围是从 1 到 11。使用 `.col-md-push-*` 和` .col-md-pull-* `类来可以互换互换两列的顺序，push向右，pull向左。

### 列嵌套

列嵌套其实很简单，就是在第一级网格系统的列中再重新添加`.row`的div，这一行同样是十二个列的宽度，同样也是父级元素的宽度。因为Bootstrap的网格系统宽度是通过百分比来确定的，也是说它是自适应的。