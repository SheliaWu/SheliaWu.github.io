---
title: Less学习笔记一
date: 2017-02-16 21:06:27
tags: Less
---

> 一直很想学习下CSS预处理语言，选择了Less，无论是Sass还是Less，选择合适自己的就好，没有优劣，下面是这几天的学习笔记。

# Less简介

开头还是老样子，介绍一下什么是Less。Less是一门CSS预处理语言，它扩充了CSS语言，增加了诸如变量、混合、函数等功能，让CSS更易维护、方便制作主题、扩充。

使用Less是编写.less后缀的文件后进行编译，编译会输出一个CSS文件。

最简单的使用方式是通过npm安装，如下：
`$npm install -g less`
安装后，就可以在命令行调用Less编译器：
`$lessc style.less>style.css`

接下来的重点是Less的语法。

# 变量

变量的格式为：@name:value，如：

    @nice-blue:#5B83AD
    @light-blue:@nice-blue + #111;
    #header{color:@light-blue;}

编译后输出为：

    #header{color:#6C94BE;}
    
- 可以在定义变量值时使用其他的变量
- 变量是按需加载的，因此不必强制在使用前声明
- 如果对同一个变量声明两次的话，在当前作用域中最后一次定义的将会被使用

# 混合（Mixins）

在Less中我们可以定义一些通用的属性集为一个class，然后在另一个class去调用这些属性。

## 直接引用某个类的全部属性

示例：

    .bordered{
    border-top:dotted 1px black;
    border-bottom:solid 2px black;
    }
    
    #menu a{
    color:#111;
    .bordered;
    }


## 引用带参数无默认值的类属性

示例：

    .border-radius(@radius){
    border-radius:@radius;
    -moz-border-radius:@radius;
    -webkit-border-radius:@radius;
    }
    
    #header{
    .border-radius(4px);
    }

## 引用带参数有默认值的类属性

    .bordered(@border_width:2px){
    border:@border_width solid black;
    }
    
    #header{.bordered();//有默认值，可以不传参}

**注意：**

- @arguments可以表示所有的参数
- @rest可以表示某个参数之后的变量，如：`.mixin(@a,@rest...){...}`，这里的@rest表示@a之后的参数。

# 模式匹配与Guard表达式

Less提供了通过参数值控制Minxin行为的功能，根据switch的值控制，示例如下：

    .mixin(dark,@color){
    color:darken(@color,10%);
    }
    .mixin(light,@color){
    color:lighten(@color,10%);
    }
    .mixin(@_,@color){
    display:block;
    }
    调用:
    @switch:light;
    .class{
    .mixin(@switch,#888);
    }
    将会得到:
    .class{
    color:#a2a2a2;
    display:block;
    }
    
**注意：**也可以根据参数的数量进行匹配

#Guards

上面的匹配为匹配值或者参数数量，Guards被用来匹配表达式。
示例：

    .mixin(@a)when(lightness(@a)>=50%){
    background-color:black;
    }
    .mixin(@a)when(lightness(@a)<50%){
    background-color:white;
    }
    .mixin(@a){
    color:@a;
    }
    
重点在于关键词when，它引入了一个Guards条件，运行以下代码：

    .class1{.mixin(#ddd)}
    .class2(.mixin(#555)}
    
输出：

    .class1{
    background-color:black;
    color:#ddd;
    }
    .class2{
    background-color:white;
    color:#555;
    }

它根据了参数的颜色值用when选择了不同的背景色。
- Guards支持的运算符包括：>,>=,<,<=,=
- 多个Guards可以通过逗号分隔，如果其中一个结果为true，则匹配成功
