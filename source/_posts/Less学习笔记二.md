---
title: Less学习笔记二
date: 2017-02-16 21:06:39
tags: Less
---
> 第二篇Less学习笔记,接着Less学习笔记一。

# 嵌套规则

在Less中可以嵌套地写CSS，如下：

    #header{
    color:black;
    .navigation{
    font-size:12px
    }
    .logon{
    width:300px;
    &:hover{text-decoration:none;}
    }
    }
    
注意&符号的使用，如果想写串联选择器可以使用，这点对伪类尤其有用：如`:hover`,`:focus`。


# 运算

任何数字、yanse或者变量都可以参与运算，运算应该被包裹在括号中。

# 函数

Less提供了很多便利的函数，想具体了解可以点击[函数手册][1]。

如`percentage(0.5)`为转化为50%。

# 命名空间

将一些变量或者混合模块打包起来，为了更好地封装，示例如下：

    #bundle{
    .button(){
    display：block;
    border:1px silid black;
    ...
    }
    .tab{...}
    ...
    }
    
你只需在#header a 中像这样引入.button:

    #header a{
    color:orange;
    #bundle > .button;
    }

# 作用域

首先会从本地查找变量或者混合模块，如果没找到的话就会去父级作用域查找，知道找到为止。

# 注释

在Less中：

 - /*...*/注释在编译成CSS会保留
 - //在编译成CSS会自动过滤掉


# 导入(Import)

 - 既可以导入CSS文件，也可以导入Less文件
 - 对CSS文件只做一处处理，将导入语句提前到前面
 - 被导入的Less文件会和当前文件共享所有的混合、变量、命名空间等
 - 导入语句是通过media query指定的，编译后包裹在@media声明中
 - Less文件的导入语句可以随意放置

# 字符串插值

变量可以用像@{name}这样的结构：

     @base-url: "http://assets.fnord.com";
    background-image: url("@{base-url}/images/bg.png");

# 避免编译

有时候我们需要输出一些不正确的 CSS 语法或者使用一些 LESS 不认识的专有语法。

要输出这样的值我们可以在字符串前加上一个 ~，例如：

    .class {
    filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
}
输出结果为：

    .class {
    filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
}

# 选择器插值

如果需要在选择器中使用变量，使用方式为`@{name}`，示例：

    @name:blocked;
    .@{name}{
    color:red;
    }
    

# media query作为变量

如果你希望在media query中使用LESS变量，你可以直接使用普通的变量方式。例如：

    @singleQuery: ~"(max-width: 500px)";
@media screen, @singleQuery {
    set {
        padding: 3 3 3 3;
    }
}
输出结果为：

    @media screen, (max-width: 500px) {
    set {
        padding: 3 3 3 3;
    }
}

结语：这两篇笔记差不多讲完了Less的语法，没有很详细，熟练还是靠多使用和练习。在前端的道路上还有很多东西要学习，just go on！！！


  [1]: http://lesscss.cn/functions/